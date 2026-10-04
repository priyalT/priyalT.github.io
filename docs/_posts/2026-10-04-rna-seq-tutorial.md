---
layout: post
title: 'What would bulk RNA-seq look like with barely 20 GB of remaining storage? <span style="color: purple;">(Part I: Raw FASTQ --> Alignment)</span>'
date: 2026-10-04
description: "An easy-to-follow tutorial on bulk RNA-seq"
tags: [rna-seq, bioinformatics]
published: true
repo: https://github.com/priyalT/snf2-rnaseq
comments: true
---

Bulk RNA-seq analysis is one of the most fundamental methods in transcriptomics. It is essentially used to quantify and compare gene expression between different conditions. Transcriptomic studies can have multiple applications, such as transcriptome assembly, refinement of gene models, metatranscriptomics, differential gene expression analysis, etc. In this series, we will walk through bulk RNA-seq analysis from raw reads to gene set enrichment analysis, all in less than 20 GB of storage. This first part takes us from raw reads to alignment.
As a student, one of the biggest obstacles in my bioinformatics analyses has been storage. I work with a MacBook Air M1 with 8 GB of RAM (ancient, I know), and I can understand when an experiment with a lot of potential falls short because of computational cost. To ensure that this doesn't end up discouraging new learners in the field, I have made this guide extremely efficient for different kinds of specs.


## Transcriptomics

RNA, or ribonucleic acid, is key in gene expression. A transcript is an RNA molecule, produced by the process of transcription (DNA --> RNA). In a broad sense, RNAs can be of two types: coding RNA and non-coding RNA, based on whether the RNA has protein-coding potential. mRNA is the major type of protein-coding RNA and serves as a template for translation to produce proteins, which is a major step in gene expression. Non-coding RNAs generally do not serve as templates for protein synthesis, but perform a variety of functions, including acting as catalysts, adaptor molecules, etc. Examples of non-coding RNA are siRNA, miRNA, tRNA, rRNA, and more.


### What will we be working on?

Within this tutorial, we will be performing differential gene expression analysis. Our samples belong to a study performed on *Saccharomyces cerevisiae* where half of the samples lack the *SNF2* gene. *SNF2* codes for the Snf2 protein, the core "engine" of a protein complex called SWI/SNF. Snf2 uses energy from ATP to slide and reposition the proteins that DNA is wrapped around (chromatin remodeling), which makes some genes easier or harder to switch on. Through this, it plays a role in DNA binding, DNA metabolic processes, and regulation of gene expression. Through this project, we aim to answer the key biological question:
> Which genes in yeast are essentially dependent on remodeling done by Snf2, and how much are they affected by its absence?

## Curating our data

We start by collecting the raw FASTQ files from [project: PRJEB5348](https://www.ebi.ac.uk/ena/browser/view/PRJEB5348?show=reads). The original dataset contains RNA-seq data from two conditions of *S. cerevisiae*, wild type vs. *snf2* knockout mutant, with 48 biological replicates per condition. Each biological replicate was sequenced on 7 different lanes (technical replicates), so the project contains 672 FASTQ files in total. A biological replicate is an independently obtained biological sample from the same experimental condition, whereas a technical replicate is a repeated measurement of the same biological sample or sequencing library, used to assess technical variability. For this experiment, I used 6 biological replicates of both conditions (12 samples in total), taking one lane from each. This keeps every file small (about 1 to 3 million reads each). One thing to keep in mind is that these files do not all come from the same lane (they range from lane 1 to lane 7). Each lane is sequenced under slightly different conditions, so the lane a sample was sequenced on can affect its data regardless of the biology. This makes lane a possible confounding variable: something other than the condition we are studying that could explain differences between samples. Ideally, we would pick all samples from the same lane, or account for the lane later in our analysis. To find out which file belongs to which condition and replicate, I used the [sample mapping file](https://github.com/bartongroup/profDGE48/blob/master/Preprocessed_data/ENAdata_ERP004763_sample_mapping.tsv) provided by the original authors, and downloaded the samples below using the following command:


| S No. | Sample | Condition |
|-------|--------|-----------|
| 1. | ERR458495 | Wild Type | 
| 2. | ERR458887 | Wild Type |
| 3. | ERR458882 | Wild Type |
| 4. | ERR458905 | Wild Type |
| 5. | ERR458906 | Wild Type |
| 6. | ERR458921 | Wild Type |
| 7. | ERR458502 | SNF2 KO  | 
| 8. | ERR458509 | SNF2 KO  |
| 9. | ERR458517 | SNF2 KO  |
| 10. | ERR458528 | SNF2 KO  |
| 11. | ERR458535 | SNF2 KO  |
| 12. | ERR458552 | SNF2 KO  |

> **Note:** ERR458535 corresponds to biological replicate 6 of the snf2 mutant, which the original authors identified as a "bad" replicate and excluded from their analysis ([Gierliński et al., 2015](https://doi.org/10.1093/bioinformatics/btv425)). I have deliberately kept it in this tutorial, and you will see it stand out as an outlier in the PCA plot in Part II. If you would rather avoid it, an unflagged mutant replicate such as ERR458524 can be used instead.

```bash
for acc in ERR458495 ERR458887 ERR458882 ERR458905 ERR458906 ERR458921 \
           ERR458502 ERR458509 ERR458517 ERR458528 ERR458535 ERR458552; do
    wget -nc "ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR458/${acc}/${acc}.fastq.gz"
done
```

If you're using macOS, you can still use wget by installing it with Homebrew. I prefer wget here because it automatically retries a download if the connection drops, and the `-nc` (no-clobber) option skips files you have already downloaded. curl can do the same with a few extra options.
> Read more: <https://daniel.haxx.se/docs/curl-vs-wget.html>

### Understanding the FASTQ format
Before we move on, let us try to understand the files we have obtained. A FASTQ file has a recognizable format that is also easy to understand:

```
@ERR458495.1 DHKW5DQ1:219:D0PT7ACXX:3:1101:2236:2048/1
CAGTTGTGACCAGATGGAGTCATTCTACCGTCCTTGACGGCCCAGACTTCT
+
CCCFFFFFHHHHHJJJJJJGIIJJJJJJJIHIIJJJJJIJIJJJJJIJIJH
```
- The first line starts with '@', followed by the identifier of the read and a further description.
- The second line contains the actual sequence of the read.
- The third line contains a '+' character.
- The fourth line contains the Phred-encoded quality scores.

> Decoding Phred quality scores is easy! First, determine the ASCII decimal value of the quality-score character and subtract 33 from it (for Phred+33 encoding). For example, the ASCII decimal value of 'C' is 67, so the Phred quality score is: 67 − 33 = 34. A Phred score of 30 corresponds to an error probability of 1 in 1,000, implying that the accuracy would be estimated to be 99.9%. Higher Phred scores indicate lower probabilities of error.


## FastQC and MultiQC
FastQC is a quality control tool for high-throughput sequencing data. We will run it on our raw sequenced reads using the following command in the directory where our raw data is present.

```
fastqc *.fastq.gz 
# or *.fastq (depending on whether the FASTQ files are gzipped or not)
```
Although it is frequently used for RNA-seq, FastQC itself is not specific to either DNA- or RNA-sequencing. Instead, it examines properties of the sequencing reads and highlights potential issues that may require further investigation.

The resulting reports contain several modules, each examining a different property of the sequencing data. Importantly, a warning or failure in FastQC does not automatically mean that the data are unusable. Some modules are particularly sensitive to the biological composition of RNA-seq libraries, so their results need to be interpreted in the context of the experiment.

Some of the modules that are particularly relevant to our dataset are:

1) **Sequence Duplication Levels:**
This module examines how frequently identical sequences occur in the dataset, that is, what percentage of sequences are duplicated, and how many times. In RNA-seq data, the number of reads from a gene reflects how highly that gene is expressed. Unlike genomic DNA sequencing, RNA-seq samples are not expected to contain uniformly distributed molecules. Highly expressed transcripts can naturally generate many reads with identical sequences. Consequently, a warning in this module should not automatically be interpreted as evidence of poor-quality RNA-seq data. *However, PCR duplication may also cause this warning, in which case it appears as elevated duplication levels uniformly across all sequences. It is more useful to consider it alongside the library preparation method, sequencing depth, and downstream alignment results.*

2) **Per Sequence GC Content:**
This module shows the distribution of GC content across individual reads and compares the observed distribution with a model based on the overall GC content of the dataset. A warning often appears for RNA-seq data because the transcriptome has a GC distribution that is different from the theoretical normal distribution (as expected genome-wide). This is because RNA-seq samples the transcriptome, and the abundance of individual transcripts influences the composition of the reads.
One can expect the distribution to be bell-shaped and centered near our organism’s expected transcriptome GC content. *A sharp, narrow peak on top of the curve usually comes from a few very highly expressed transcripts or leftover rRNA, while a second broad peak can suggest contamination from another organism.*

3) **Per Base Sequence Quality:**
This module examines the quality of bases at each position across the reads. FastQC summarizes the distribution of Phred quality scores at every position, allowing us to identify positions where sequencing quality deteriorates.
For Illumina data, it is common for quality to decrease toward the 3′ end of reads. Read 2 in paired-end sequencing can also have lower quality than Read 1. These patterns are not inherently problematic; what matters is the magnitude and consistency of the deterioration. A substantial loss of quality toward the end of the reads may indicate that trimming would be beneficial before downstream analysis. On the other hand, a small decline in quality does not necessarily require trimming, particularly if the remaining bases have sufficient quality for the intended analysis. The green, orange, and red regions in FastQC are useful visual indicators, but they should not be treated as strict biological or sequencing-quality thresholds. The actual quality scores, read length, sequencing platform, and downstream requirements should all be considered. *If the median quality collapses sharply, that indicates a problem with the sequencing run.*

Apart from these, our dataset contains two additional FastQC results worth discussing:

1) **Per Base N Content:**
For two of our samples (mutant), the 6th base shows 6% N content. An N represents a base that could not be confidently assigned during sequencing. The FastQC result therefore tells us that a small but noticeable fraction of reads contain an undetermined base at this position (FastQC raises a warning once this goes above 5%). However, FastQC alone cannot establish the cause of this pattern. We should therefore treat it as a characteristic of these sequencing libraries and investigate the original sequencing/run metadata before attributing it to a particular technical cause.

2) **Per Tile Sequence Quality:**
Our dataset fails on this module. Illumina flow cells are divided into physical regions called tiles. This FastQC module examines whether sequencing quality differs systematically between these tiles. A localized decrease in quality can indicate a spatial or technical problem associated with the sequencing run. A failed Per Tile Sequence Quality module therefore does not, by itself, mean that the entire dataset is unusable. The plot shows, for each tile, how much its quality differs from the average of all tiles. Problems here usually come from the flow cell during that particular sequencing run (for example, air bubbles or debris on the flow cell), rather than from the biology of our samples. Overall, FastQC should be viewed as a diagnostic tool rather than a simple pass/fail test. 

MultiQC allows us to aggregate our FastQC results and to compare how all of our samples perform on the FastQC modules. We run MultiQC in the current working directory (which contains all of our FastQC reports) using the following command:

```
multiqc .
```

## Trimming

Trimming is mainly used to remove or "trim" adapter sequences and, when required, low-quality bases from sequencing reads. This can improve the quality of the reads used in downstream analyses. In our FastQC reports, we did not see evidence of substantial adapter contamination. Therefore, we do not expect adapter trimming to have a major effect on this particular dataset. Nevertheless, we will use Trim Galore to perform adapter and quality trimming using its default settings.

For our dataset (single-end), we can use:

```
trim_galore --fastqc --gzip *.fastq.gz

# or if they are uncompressed: trim_galore --fastqc --gzip *.fastq
```
Trim Galore uses Cutadapt for the actual trimming. By default, Trim Galore asks Cutadapt to trim low-quality bases (Phred score below 20) from the 3' end of each read, and to remove adapter sequences whenever it detects them. Trim Galore also discards any read that becomes shorter than 20 bp after trimming.

The *--fastqc* option tells Trim Galore to run FastQC on the trimmed output, allowing us to immediately compare the quality of the reads before and after trimming. We can then aggregate the resulting reports with MultiQC. For this dataset, we expect trimming to have only a modest effect because the FastQC reports do not indicate substantial adapter contamination. The most noticeable change may therefore be in the read-length distribution, particularly at the lower end, as low-quality bases are removed and reads that become too short are discarded.

## Alignment and Mapping
To understand our dataset in the context of our organism's genome, we need to work out where in the genome each read came from. This step is called alignment, or mapping (the two terms are often used interchangeably). We will be using the STAR aligner, which is a splice-aware aligner. This means it can handle reads that are split across two exons, because the intron in between has been spliced out of the mRNA. Yeast has relatively few introns (only about 5% of its genes contain one), so most of our reads will align in one piece, but using a splice-aware aligner is still good practice for RNA-seq. This step will become clearer as we follow along.
First, we need to create an index for our organism's genome. An index is a pre-processed version of the reference genome that lets STAR search it very quickly, a bit like the index at the back of a book. We only need to do this step once for a given genome and annotation. To create the index, we require a yeast genome sequence file (FASTA) and a GTF (Gene Transfer Format) annotation file. STAR uses the GTF to learn where genes and splice junctions are, which it also needs to count reads per gene later on. I downloaded both from [Ensembl](https://asia.ensembl.org/Saccharomyces_cerevisiae/Info/Index), which provides the reference genome (assembly R64-1-1) of the standard laboratory yeast strain S288C, along with a ready-to-use GTF file. Whichever source you use, make sure both files come from the same place and release, so that the chromosome names match.

> A GTF file is a tab-delimited file which is a specialized version of a GFF (General Feature Format) file and consists of 9 columns, like chromosome name, start and end coordinates, strand orientation, etc. It is very helpful in gene annotation and describes where genomic features (genes, transcripts, exons, etc.) are located.

We create the index using:
```
mkdir -p star_index   # create a new directory for the index

# --genomeDir: output directory of your index
# --genomeFastaFiles: name of your genome file
# --sjdbGTFfile: name of your genome annotation GTF file
# --sjdbOverhang: read length - 1 (our reads are 51 bp long)
# --genomeSAindexNbases: controls the size of STAR's index; a smaller value reduces memory requirements (needed for small genomes like yeast)
STAR --runMode genomeGenerate \
     --runThreadN 4 \
     --genomeDir star_index \
     --genomeFastaFiles genome.fa \
     --sjdbGTFfile annotation.gtf \
     --sjdbOverhang 50 \
     --genomeSAindexNbases 11
```
This creates an index, which we will use for our mapping and alignment. We perform mapping and alignment using the following:

```
# Trim Galore names its output files like ERR458495_trimmed.fq.gz
# --genomeDir: directory of your generated index
# --readFilesIn: path to your trimmed reads
# --readFilesCommand: how to decompress the gzipped files (gunzip -c works on both macOS and Linux)
# --outFileNamePrefix: output prefix, so each sample gets its own files
# --quantMode GeneCounts: count how many reads fall on each gene
# --outSAMtype None: don't write BAM/SAM files, to save storage
for reads in *_trimmed.fq.gz; do
    sample=${reads%_trimmed.fq.gz}
    STAR --genomeDir star_index \
         --readFilesIn "$reads" \
         --readFilesCommand gunzip -c \
         --outFileNamePrefix "${sample}_" \
         --quantMode GeneCounts \
         --outSAMtype None \
         --runThreadN 4
done
```

Usually, the alignment produces BAM/SAM files as output. They take up a lot of space, and because we want to build a storage-efficient pipeline, we'll skip them and use `--quantMode GeneCounts` instead. This will produce a ReadsPerGene.out.tab file for each sample, which we will be using to create our count matrix. This file has four columns: the gene ID, followed by three columns of read counts. The three count columns differ in how they treat the strand the read came from, and we will pick the right one in Part II. Either way, it is important to know what a BAM/SAM file is and what it includes.

A SAM (Sequence Alignment/Map) file is an informative file providing us insight into the mapping, position, alignment, etc. of our reads to the genome. A BAM (Binary Alignment Map) file is a compressed, binary version of the SAM file that takes up much less space but is not human-readable. The following is a broad overview of the contents of a BAM/SAM file:

### Header section:
The header section is an optional section within the file and usually contains the metadata related to the alignment. It can include versioning info, commands used to generate the file, information about the genome, etc.

### Alignment section:
1. QNAME = Query name; this refers to the name of the read which was mapped.
2. FLAG = A number that encodes several yes/no facts about the alignment (for example: whether the read is mapped, whether it aligned to the reverse strand, or whether it is part of a pair)
3. RNAME = Name of the reference to which the read was mapped (for example: chrI or I)
4. POS = 1-based leftmost mapping position with respect to reference, e.g. a POS of 100 would indicate the read mapping starts at the 100th position of RNAME.
5. MAPQ = An integer which reflects how confident the aligner is that the read was placed in the right location (higher is better)
6. CIGAR = A string of letters and numbers describing, from left to right, how the read lines up with the reference: matches (M), insertions (I), deletions (D), and skipped regions (N). For example, 20M500N31M means 20 bases align, then 500 bases of the reference are skipped, then 31 more bases align. In RNA-seq, an N usually means the read spans an intron. Note that M covers both matching and mismatching bases.
7. RNEXT = Reference name of the mate read (relevant for paired-end data)
8. PNEXT = Mapping position of the mate read (relevant for paired-end data)
9. TLEN = Template length (relevant for paired-end data)
10. SEQ = The sequence of bases constituting the underlying read.
11. QUAL = The sequence of ASCII characters encoding the Phred quality scores for each base of the underlying read.
12. Further optional fields

More information regarding the format can be found in the [SAM specification](https://samtools.github.io/hts-specs/SAMv1.pdf).

The next steps are to construct a count matrix, perform further analysis, and wrap our pipeline within Nextflow. I will be covering those in Part II and Part III. Stay tuned!

<div class="references" markdown="1">

## Resources and Additional Reading

+ <https://www.ncbi.nlm.nih.gov/gene/854465>
+ Gierliński et al. (2015). Statistical models for RNA-seq data derived from a two-condition 48-replicate experiment. *Bioinformatics*. <https://doi.org/10.1093/bioinformatics/btv425>
+ Schurch et al. (2016). How many biological replicates are needed in an RNA-seq experiment and which differential expression tool should you use? *RNA*. <https://doi.org/10.1261/rna.053959.115>
+ <https://daniel.haxx.se/docs/curl-vs-wget.html>
+ <https://www.biostars.org/p/218995/>
+ <https://training.galaxyproject.org/training-material/topics/transcriptomics/tutorials/rna-seq-bash-star-align/tutorial.html#alignment-to-a-reference-genome>
+ <https://olvtools.com/en/documents/star>
+ <https://hbctraining.github.io/Training-modules/planning_successful_rnaseq/#part-i>

</div>
