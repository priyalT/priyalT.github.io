---
layout: post
title: 'What would bulk RNA-seq look like with barely 20 GB of remaining storage? <span style="color: purple;">(Part II: Count Matrix --> Differential Expression)</span>'
date: 2026-10-05
description: "An easy-to-follow tutorial on bulk RNA-seq"
tags: [rna-seq, bioinformatics]
published: true
repo: https://github.com/priyalT/snf2-rnaseq
comments: true
---

Hello! Welcome to the second part of my tutorial series on performing bulk RNA-seq analysis with limited computational hardware. This tutorial series focuses on building an understanding of each step in a general bulk RNA-seq pipeline, through a hands-on project which does not require a lot of RAM and storage. For the tutorial on how to go from raw FASTQ reads to alignment and mapping, check out the [Part I tutorial](https://priyalt.github.io/2026/10/04/rna-seq-tutorial.html). This part of our tutorial series is gonna be focused more on the theoretical aspects of differential expression.

## Overview of Previous Steps
We started with our reproducible RNA-seq project by taking a look at the data [(project: PRJEB5348)](https://www.ebi.ac.uk/ena/browser/view/PRJEB5348?show=reads) and understanding the biological question we are trying to answer. We proceeded with curating our data, understanding the FASTQ format, performing FastQC on our reads and aggregating our FastQC reports using MultiQC. We also worked through some modules that are often flagged in these QC reports for RNA-seq data (Sequence Duplication Levels, Per Sequence GC Content, Per Base Sequence Quality). We then moved on to understanding the concept, purpose and mechanism behind trimming. We used Trim Galore with default settings. Lastly, we created an index for yeast and aligned our reads to the genome index using STAR Aligner while understanding the splice-aware alignment performed by STAR.

## Generating a Count Matrix
A count matrix is a data structure that contains the genes as rows and samples as columns. Our STAR Aligner run produced a 'ReadsPerGene.out.tab' file. This tab-delimited file contains four columns:
- column 1: gene ID
- column 2: counts for unstranded RNA-seq
- column 3: counts for 1st read strand
- column 4: counts for 2nd read strand

> Strandedness is an important concept in RNA-seq. It refers to how the sequencing data was generated, and whether it retains information about the original orientation (strand) of the RNA molecules from which the reads were generated. This information is determined by the library preparation method. In a stranded RNA-seq library, the orientation of the original RNA molecule is preserved, allowing us to determine whether a read originated from the sense or antisense strand. In an unstranded library, this information is lost, so it is not possible to reliably determine the original RNA strand from the read alone. Importantly, single-end and paired-end sequencing can both be either stranded or unstranded. Therefore, when analysing RNA-seq data, it is important to know whether the library is stranded and, if it is stranded, which strand-specificity convention was used.

These columns are generated based on assumptions: is our RNA-seq data unstranded or stranded, and if stranded, then which strand?

For our dataset, we will utilise column 2 for our gene counts, because our data is unstranded. We will use R to create our count matrix. The code for this step mostly uses basic R libraries and packages, and I'd like for you to try and figure out how to create the count matrix. You can also find the code at my [GitHub](https://github.com/priyalT/snf2-rnaseq/blob/main/workflow/bin/count_matrix.R).

The count matrix results in a total of 7127 genes (as rows) and 12 samples (as columns).

We can try exploring our count data using different basic R functions. We could see how many genes have no expression across all samples (about 8.2%) or what the range of our library sizes is, etc. I'll leave the exploration to you as well; I hope you have fun looking around the data and understanding what it means.

We can also see how the GTF file that we procured earlier for yeast is relevant to our count matrix. The GTF file helps in splice-aware alignment during the reference genome index creation (as we discussed earlier). During alignment too, STAR uses GTF files to position genes to their particular places. While a GTF file is not absolutely necessary for alignment, it will affect our count matrix. Loss of the GTF file would lead to an incorrect number of genes within our count matrix, and we would lose important biological information.

## DESeq2
DESeq2 is a Bioconductor package used to perform differential gene expression analysis based on the negative binomial distribution.

### Why the Negative Binomial Works for Count Data
To visualise and simplify large datasets, plotting curves is a common practice. For gene count data specifically, the density curve shows us quite a few things:
- The distribution of the plot is not bell-shaped
- Only whole numbers are present (no decimals)
- Data is discrete and not continuous

We cannot plot a normal distribution curve. Hence, we look into alternatives to help us understand our dataset.
One of the alternatives is the Poisson distribution. According to the definition:

> The Poisson distribution is a discrete probability distribution that gives the probability of exactly k events occurring in a fixed interval — of time, space, area, or volume — when events happen independently at a constant average rate λ (lambda).

Essentially, it is important to understand that the Poisson distribution is especially used when the number of cases is high (number of reads) and the probability of an event occurring, say a read being mapped to a gene, is low. Using a Poisson distribution for our dataset helps us estimate the parameters like mean and probability of an event occurring.

![Poisson Distribution](/assets/images/rna-seq/PoissonDistribution.png)

One characteristic of a Poisson distribution is that the mean (lambda) = variance for the dataset. Extrapolating it to our RNA-seq dataset, we can see that that is not true:

![RNA-seq mean vs. variance](/assets/images/rna-seq/deseq2_mean_variance.png)
*Image source: [HBC Training](https://hbctraining.github.io/)*

This is due to variability in variance for our biological replicates. This is where the negative binomial enters the picture. A negative binomial distribution is able to fix this drawback of the Poisson distribution, and accounts for extra variability by including dispersion and fitted mean in its modelling. It also takes sample-specific size factors into account.

### How DESeq2 Essentially Works
DESeq2 performs four essential processes:
1. It estimates the size factor for each sample
2. It estimates dispersion for each gene
3. It fits the linear model for each gene
4. It utilises hypothesis testing to generate a p-value, to determine if a gene is differentially expressed or not.

**Estimation of size factors:**
Due to library preparation and many other factors, there can be biases introduced in our gene counts data. To observe a genuine biological effect, we need to make sure that the biases introduced by technicalities and the preparation of our data are accounted for. DESeq2 accounts for the following two biases by estimating a size factor for each sample:
- *Library size:* Sequencing depth can cause samples to have different amounts of mapped reads
- *Library composition:* If a few genes are very highly expressed in one sample, they take up a large share of that sample's reads, which makes every other gene in that sample look lower than it actually is.

Thus, a size factor is estimated using the **median of ratios method** for each sample to account for the biases.

1. Calculate the geometric mean (the geometric mean is more robust to outliers, and therefore more reliable) of a gene's counts across the samples
2. Divide the counts by the geometric mean, per sample
3. Take the median of these ratios for each sample
4. This median becomes the normalisation factor, so now the count values are normalised using this factor (counts/normalisation factor)

**Estimation of dispersion:**
Dispersion, by definition, refers to the measure of how values in a dataset are spread out or scattered around an average (mean). In simpler terms, it measures how far a data value is from the dataset's mean value. As we discussed before, biological replicates have variability, which leads to the mean and variance being unequal. Dispersion estimates calculated by DESeq2 will account for that.
Dispersion estimates are calculated using the **maximum likelihood estimation** method.

1. The dispersion estimates (maximum likelihood estimation) are plotted against the mean of normalised counts.
2. A curve is fit to the gene-wise dispersion estimates.
3. The gene-wise dispersion estimates are then shrunk towards the values predicted by the curve.

Genes with extremely high dispersion values are not shrunk to the curve, because DESeq2 assumes that they have variability other than biological and technical, and they cannot be accounted for.

**Fitting the Linear Model:**
Based on the design factor, a generalized linear model (GLM) is fit to the data per gene. This leads to the calculation of the logFC, or log fold change. This logFC is the differential expression of our gene between conditions.

**Hypothesis Testing:**
Within most experiments, a null hypothesis is generated, and the aim is to reject the null hypothesis. The null hypothesis states that the causality we are trying to study does not exist. In our case, it means that there is no differential expression between two sample groups, or logFC = 0. The Wald test is a statistical test that helps provide evidence that logFC calculated by our GLM across genes is not 0. DESeq2 runs the Wald test.

### DESeq2 workflow

We begin our DESeq2 analysis by first loading relevant libraries:

```r
library(DESeq2)
library(ggplot2)
library(apeglm)
```
We will then read our count matrix as well as the samplesheet (containing sample names and information about their condition, which can be extracted from the project website).

```r
counts_data <- read.csv("countmatrix.csv", header=TRUE, row.names="X")

colData <- read.csv("samplesheet.csv", header=TRUE, row.names="sample")
```
Our samplesheet contains two columns: the sample names and their condition (WT or snf2). Before moving on, it is important to check that the row names of our samplesheet match the column names of our count matrix, *in the same order*. DESeq2 uses this to map each column of the count matrix to its sample information, so if they don't match, DESeq2 will throw an error (or worse, our samples could get mislabelled). We can check this using:

```r
all(rownames(colData) %in% colnames(counts_data)) # are all the sample names present in both?
all(rownames(colData) == colnames(counts_data))   # are they in the same order?
```
Both should return `TRUE`. If the first is `TRUE` but the second is `FALSE`, we can reorder our count matrix using `counts_data <- counts_data[, rownames(colData)]`.

We will move on to create our DESeq dataset from our count matrix:

```r
dds <- DESeqDataSetFromMatrix(countData = counts_data, #count matrix
                              colData = colData, #samplesheet
                              design = ~ condition) #design factor, i.e. which variable are we basing our experiment on
```
The design formula tells DESeq2 which variable our experiment is based on. Here, our design factor is the `condition` column, so DESeq2 will measure the log fold change between our conditions (WT and snf2). If our experiment had a second variable, say the sequencing batch, we would add it with a `+`, like `~ batch + condition`. This way, DESeq2 controls for the batch, and any variation caused by it isn't mistaken for an effect of our condition. By convention, the variable we are interested in goes last.

Next, we filter out genes with very low counts:

```r
keep <- rowSums(counts(dds)) >= 10 #keep the genes that have 10 or more reads in total, across all samples
dds <- dds[keep, ]
```
`rowSums` adds up the reads for each gene across all our 12 samples. So, a gene with 3 reads in one sample and 0 in the rest has a total of 3, and gets filtered out, while a gene with 1 read in every sample has a total of 12, and is kept. Note that this filter only looks at the total, and not at how the reads are spread across the samples. A stricter alternative is to keep genes that have at least 10 reads in at least *n* samples, where *n* is the size of our smallest group (6 in our case, as we have 6 WT and 6 snf2 samples):

```r
keep <- rowSums(counts(dds) >= 10) >= 6
```
Here, `counts(dds) >= 10` gives us a TRUE/FALSE table, and since R counts TRUE as 1, `rowSums` counts the number of samples that passed. Choosing the smallest group size makes sure that we don't lose a gene that is switched on in only one condition (for example, expressed in all 6 snf2 samples, and silent in WT).

But why filter at all? A gene with only a handful of reads across all samples can practically never be called differentially expressed; there just isn't enough data. Keeping thousands of such genes means thousands of extra tests (which, as we will see below, makes our multiple testing correction harsher for all genes), noisy dispersion estimates, and more time and memory spent on our already limited hardware.

We then set our reference level:

```r
dds$condition <- relevel(dds$condition, ref="WT") #use the wild type condition as the baseline
```
`relevel` makes WT the baseline from which our log fold change will be measured, so a positive log2FoldChange means a gene is upregulated in the snf2 mutant compared to WT. This has to be done before running DESeq2. If we don't set it ourselves, R picks the reference alphabetically, and which of "snf2" and "WT" comes first can actually depend on your computer's locale settings (you can check it with `levels(factor(c("snf2", "WT")))`). So, it is always better to set the reference explicitly rather than rely on the default.

Finally, we run DESeq2:

```r
dds <- DESeq(dds) #run deseq2
```
This one line performs all four steps we discussed above, and we can see them in the messages it prints:

```
estimating size factors        --> step 1: size factor estimation
estimating dispersions         --> step 2: dispersion estimation
gene-wise dispersion estimates --> 2.1: dispersion estimates per gene
mean-dispersion relationship   --> 2.2: fitting the curve
final dispersion estimates     --> 2.3: shrinking towards the curve
fitting model and testing      --> steps 3 & 4: GLM fitting + Wald test
```

We can now look at the results of our DESeq2 run. We set our alpha to 0.05. Alpha is the probability of rejecting the null hypothesis when it is actually true, i.e. it limits false positives (calling a gene differentially expressed when it isn't). A lower alpha value makes our experiment stricter.

Our results contain both a `pvalue` and a `padj` (adjusted p-value) column. Why do we need an adjusted p-value? Because we are not running one test, we are running one per gene, 6,115 of them (the genes that survived our filtering). At an alpha of 0.05, even if snf2 changed nothing at all, we would still expect about 6,115 × 0.05 ≈ 306 genes to look "significant" purely by chance! This is the problem of multiple hypothesis testing. DESeq2 handles it using the Benjamini-Hochberg method, which controls the **false discovery rate (FDR)**: if we keep the genes with padj < 0.05, we expect about 5% of that list to be false positives. For our 1,372 significant genes, that is around 69 false positives. (This is also why filtering low-count genes helps: fewer pointless tests means a less harsh correction.)

We then move on to plotting a basic MA plot using ggplot.

```r
  res <- results(dds, alpha = 0.05)
  resdf <- as.data.frame(res)
  resdf$gene <- rownames(resdf)
  resdf$sig <- !is.na(resdf$padj) & resdf$padj < 0.05
  
  top_genes <- head(resdf[order(resdf$padj), ], 10)

  ma_plot <- ggplot(resdf, aes(x = baseMean, y = log2FoldChange, colour = sig)) +
    geom_point(alpha = 0.4, size = 0.8) +
    scale_x_log10() +
    geom_hline(yintercept = 0, color = "black", linewidth = 0.5) +
    scale_colour_manual(values = c("grey70", "#0073C2FF"), labels = c("Not significant", "Significant")) +
    guides(colour = guide_legend(override.aes = list(size = 3, alpha = 1))) +
    geom_text(data = top_genes, aes(label = gene), size = 3, show.legend = FALSE, vjust = -0.5, check_overlap = TRUE) +
    labs(title = "MA Plot (Unshrunken)", x = "Mean of Normalized Counts", y = "Log2 Fold Change", colour = "Status")
  
  ggsave("deseq2_output/MA_plot.png", plot = ma_plot, width = 8, height = 6)
```
![MA plot (unshrunken)](/assets/images/rna-seq/MA_plot.png)

An MA plot shows the mean of normalised counts for each gene on the x-axis (how highly expressed a gene is), and its log2 fold change on the y-axis (how much it changed between snf2 and WT). Genes above 0 are upregulated in snf2, and genes below 0 are downregulated. The coloured dots are our significant genes (padj < 0.05).

You might expect that the genes furthest away from 0 are always the significant ones, but our plot shows that this isn't the case. On the far left (mean counts below ~5), there are grey dots with log2 fold changes of +2.5 to +3, which are not significant, while on the right (mean counts of ~500 to 2000), there are blue dots with log2 fold changes of only around ±0.5, which are. Significance depends not just on how big the fold change is, but also on how many reads support it. A gene with 2 reads in WT and 8 reads in snf2 has a log2 fold change of 2, but that could easily be random sampling noise. A gene going from 2,000 to 8,000 reads has the same fold change, but is far more convincing. This is why low-count genes are rarely significant: they just don't have enough reads to tell a real change apart from noise.

We then move on to making the PCA plot:

```r
  vsd <- vst(dds, blind = FALSE)
  
  pca_data <- plotPCA(vsd, intgroup = "condition", returnData = TRUE)
  percentVar <- round(100 * attr(pca_data, "percentVar"))
  
  pca_plot <- ggplot(pca_data, aes(PC1, PC2, color = condition, label = name)) +
    geom_point(size = 3) +
    geom_text(size = 3, show.legend = FALSE, check_overlap = TRUE, vjust = -0.5) +
    guides(color = guide_legend(override.aes = list(size = 4))) +
    labs(
      title = "PCA: snf2 (KO) vs WT",
      x = paste0("PC1: ", percentVar[1], "% variance"),
      y = paste0("PC2: ", percentVar[2], "% variance")
    )
  ggsave("deseq2_output/PCA_plot.png", plot = pca_plot, width = 8, height = 6)
```
Before plotting our PCA, we transform our counts using `vst()`, the **variance stabilizing transformation** (`vsd` is just the name of our variable, short for "variance-stabilized data"). Why not use the raw counts directly? As we saw earlier, in count data the variance grows with the mean, so the most highly expressed genes would dominate our PCA. VST works roughly like a log transformation, and makes the variance about constant across expression levels, so that every gene gets a fair say. We set `blind = FALSE`, which lets the transformation use our design (`~ condition`) when estimating the dispersion trend. `blind = TRUE` would ignore the design completely, and is the fully unbiased choice if you are only doing quality control.

![PCA plot of WT and snf2 samples](/assets/images/rna-seq/PCA_plot.png)

In our PCA plot, PC1 explains 64% of the variance and cleanly separates WT from snf2. This means that the biggest source of variation in our data is our condition: the knockout has a strong effect, and the replicates within each group are consistent. One of the snf2 samples, ERR458535, sits far away from the other snf2 samples. According to the original authors, this is a bad replicate.

Next, we shrink our log fold change values, and make an MA plot after that.

```r
  resLFC <- lfcShrink(dds, coef = "condition_snf2_vs_WT", type = "apeglm", res = res)
  
  shrunkenresdf <- as.data.frame(resLFC)
  shrunkenresdf$gene <- rownames(shrunkenresdf)
  shrunkenresdf$sig <- !is.na(shrunkenresdf$padj) & shrunkenresdf$padj < 0.05
  top_genes_shrunken <- head(shrunkenresdf[order(shrunkenresdf$padj), ], 10)
  shrunken_ma_plot <- ggplot(shrunkenresdf, aes(x = baseMean, y = log2FoldChange, colour = sig)) +
    geom_point(alpha = 0.4, size = 0.8) +
    scale_x_log10() +
    geom_hline(yintercept = 0, color = "black", linewidth = 0.5) +
    scale_colour_manual(values = c("grey70", "#0073C2FF"), labels = c("Not significant", "Significant")) +
    guides(colour = guide_legend(override.aes = list(size = 3, alpha = 1))) +
    geom_text(data = top_genes_shrunken, aes(label = gene), size = 3, show.legend = FALSE, vjust = -0.5, check_overlap = TRUE) +
    labs(title = "MA Plot (Shrunken)", x = "Mean of Normalized Counts", y = "Log2 Fold Change", colour = "Status")
  
  ggsave("deseq2_output/MA_shrunkenLFC_plot.png", plot = shrunken_ma_plot, width = 8, height = 6)
  
  
  png("deseq2_output/compare_shrunkenLFC_plot.png", width = 800, height = 600)
  plot(
    res$log2FoldChange,
    resLFC$log2FoldChange,
    xlab = "Unshrunken LFC",
    ylab = "Shrunken LFC"
  )
  abline(0, 1, col = "red")
  dev.off()

  resdf <- as.data.frame(resLFC)
  resdf$sig <- !is.na(resdf$padj) & 
              resdf$padj < 0.05 & 
              abs(resdf$log2FoldChange) > 1

  resdf$gene <- rownames(resdf)

```
![MA plot (shrunken)](/assets/images/rna-seq/MA_shrunkenLFC_plot.png)

![Shrunken vs. unshrunken log fold changes](/assets/images/rna-seq/compare_shrunkenLFC_plot.png)

Shrinkage doesn't normalise our data (our counts were already normalised using the size factors); it deals with **noise**. Just like in the MA plot, a low-count gene can get a huge log fold change purely by chance. `lfcShrink` with `apeglm` pulls each log fold change towards 0, in proportion to how *uncertain* that estimate is. We can see this in our comparison plot: most genes sit on the red line, meaning that well-measured genes barely move, while some genes with unshrunken log fold changes of ±2 to 3 get pulled close to 0. These are mostly genes with low counts and high dispersion. This gives us log fold changes that we can actually trust for ranking our genes, which is exactly what we will need for GSEA in Part III.

We then make a volcano plot. Note that from here on, `resdf` contains our **shrunken** log fold changes (unlike our first MA plot), and we now call a gene significant only if it has padj < 0.05 *and* an absolute log2 fold change greater than 1 (i.e. at least a 2-fold change).

```r
  top_genes_volcano <- head(resdf[order(resdf$padj), ], 10)
  volcano <- ggplot(resdf, aes(x = log2FoldChange, y = -log10(padj), colour = sig)) +
    geom_point(alpha = 0.4, size = 0.8) +
    scale_colour_manual(values = c("grey70", "#c0392b"), labels = c("Not significant", "Significant")) +
    guides(colour = guide_legend(override.aes = list(size = 3, alpha = 1))) +
    geom_vline(xintercept = c(-1, 1), linetype = "dashed", color = "black", linewidth = 0.3) +
    geom_hline(yintercept = -log10(0.05), linetype = "dashed", color = "black", linewidth = 0.3) +
    geom_text(data = top_genes_volcano, aes(label = gene), size = 3, show.legend = FALSE, vjust = -0.5, check_overlap = TRUE) +
    labs(
      title = "Volcano Plot: snf2 (KO) vs WT",
      subtitle = paste0(sum(resdf$sig), " significant genes (padj < 0.05 & |LFC| > 1)"),
      colour = "padj < 0.05 & |LFC| > 1"
    )
  ggsave("deseq2_output/Volcano_plot.png", plot = volcano, width = 8, height = 6)
```
![Volcano plot](/assets/images/rna-seq/Volcano_plot.png)

A volcano plot shows the log2 fold change on the x-axis, and the significance (-log10 of padj) on the y-axis, so the most significant genes are the highest up. The dashed vertical lines mark our fold change cutoff (±1), and the dashed horizontal line marks padj = 0.05. We use padj on the y-axis rather than the raw p-value, so that the dashed line and our colours agree with each other. If we used the p-value instead, a gene with p < 0.05 but padj > 0.05 would sit above our line and still be grey. Out of our 1,372 genes with padj < 0.05, 396 also pass our fold change cutoff. We also calculate the number in the subtitle from our data, instead of typing it in ourselves, so that it always matches the plot.

Finally, we make a heatmap of our top 50 differentially expressed genes, and save our results:

```r
# Heatmap of top 50 differentially expressed genes (base R stats::heatmap)
  top50_genes <- head(rownames(resdf[order(resdf$padj), ]), 50)
  mat <- assay(vsd)[top50_genes, ]
  mat <- mat - rowMeans(mat)
  
  col_side_colors <- ifelse(colData$condition == "WT", "#4DAF4A", "#E41A1C")
  
  png("deseq2_output/Heatmap_top50.png", width = 800, height = 1000)
  heatmap(mat,
          scale = "none",
          ColSideColors = col_side_colors,
          col = colorRampPalette(c("#0073C2FF", "white", "#E41A1C"))(50),
          main = "Top 50 DE Genes (snf2 knockout vs WT)",
          margins = c(8, 8))
  dev.off()

  write.csv(as.data.frame(resLFC[order(resLFC$padj), ]),
            "deseq2_output/de_snf2_vs_WT.csv")

```

![Heatmap of the top 50 differentially expressed genes](/assets/images/rna-seq/Heatmap_top50.png)

A heatmap lets us look at the expression of our top genes across every single sample at once. Here, we pick the 50 genes with the lowest padj, and there are a few things worth understanding about how we build it:

- **Why `vsd` and not raw counts?** For the same reason as our PCA: in raw counts, highly expressed genes have much larger values (and variance) than lowly expressed ones, so they would dominate the colour scale. The variance stabilized data puts all genes on a comparable, roughly log scale.
- **Why subtract `rowMeans(mat)`?** Even after VST, some genes are simply expressed at higher levels than others. If we plotted the values as they are, a highly expressed gene would look "hot" in every sample, and a lowly expressed one "cold" in every sample, hiding the differences we actually care about. By subtracting each gene's mean across all samples (this is called *centring*), every row is now centred around 0, and the colours show how much a gene's expression in a sample is **above (red) or below (blue) that gene's own average**. We set `scale = "none"` because we have already done our own centring, and don't want `heatmap()` to rescale the rows again.
- **The coloured bar on top** marks the condition of each sample: red for snf2, green for WT.
- **The trees (dendrograms)** on the top and left come from hierarchical clustering: samples (and genes) with similar expression patterns are placed next to each other.

Looking at our heatmap, the samples cluster cleanly into two groups that match our conditions exactly, without us telling the clustering anything about the conditions, which agrees with what we saw in our PCA. Most of our top 50 genes are blue in snf2 and red in WT, i.e. they are downregulated in the snf2 mutant, while a few genes at the bottom (such as YER081W) show the opposite pattern and are upregulated. This matches our volcano plot, where most of our top genes had negative log fold changes, and YER081W was one of the few upregulated ones.

## Conclusion

With this, we wrap up DESeq2's expression analysis. In the next part of the tutorial series, we will be covering GSEA. Hope to see you there!

<div class="references" markdown="1">

## Resources and Additional Reading

+ <https://mlspring.beehiiv.com/>
+ <https://hbctraining.github.io/>

</div>