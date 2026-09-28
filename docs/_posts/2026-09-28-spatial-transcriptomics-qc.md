---
layout: post
title: "How would bulk rna-seq look like with barely 20GB of remaining storage?"
date: 2026-09-28
description: "An easy-to-follow tutorial on Bulk RNA-seq"
tags: [rna-seq, bioinformatics, python]
---

Bulk RNA-seq analysis is one of the most fundamental methods in transcriptomics. It is essentially used to quantify gene expression between different conditions. Studying transcriptomics could have multiple results, like transcriptome assembly, refinement of gene models, metatranscriptomics, differential gene expression analysis, etc. In this tutorial, we will walk through bulk RNA-seq analysis from raw reads to gene set enrichment analysis, all in less than 20 GB.
As a student, one of the most key obstacles in my bioinformatics analyses has been storage. I work with an 8 GB Macbook Air M1 (ancient, I know), and I can understand when an experiment with a lot of potential falls short because of computational cost. To ensure that this doesn't end up discouraging new learners in the field, I have made this guide extremely efficient for different kinds of specs.


## Transcriptomics

RNA, or ribonucleic acid, is key in gene expression. A transcript is an RNA molecule, produced by the process of transcription (DNA --> RNA). In a broad sense, RNAs can be of two types: coding RNA and non-coding RNA, based on whether the RNA has protein-coding potential. mRNA is the major type of protein-coding RNA, and serves as a template for translation to produce proteins, which is a major step in gene expression. Non-coding RNAs generally do not serve as templates for protein synthesis, but perform a variety of functions, including acting as catalysts, adaptor molecules, etc. Examples of non-coding RNA are siRNA, miRNA, tRNA, rRNA, and more.


### What will we be working on?

Within this tutorial, we will be performing differential gene expression analysis. Our samples belong to a study performed on *Saccharomyces cerevisiae* where half of the samples lack the snf2 gene. The snf2 gene is responsible for multiple functions, especially including ATP-dependent chromatin remodeler activity. It is also a part of the SWI/SNF complex, and contributes to DNA binding activity, DNA metabolic processes, and regulation of gene expression. Through this project, we maintain the key biological question: 
> Which genes in yeast are essentially dependent on remodelling done by snf2, and how much are they affected by the absence of snf2?

## Curating our data

We start by collecting the raw fastq files from [project: PRJEB5348](https://www.ebi.ac.uk/ena/browser/view/PRJEB5348?show=reads). The original dataset has 48 biological and 7 technical replicates of two conditions: wild type vs. snf2 knockout mutant RNA-seq of *S. cerevisiae*. 

1. **Total UMI Counts per Spot (`total_counts`)**: Identifies sequencing saturation and low-permeabilization regions.
2. **Detected Genes (`n_genes_by_counts`)**: Evaluates transcript complexity per spot.
3. **Mitochondrial Read Percentage (`pct_counts_mt`)**: Elevated rates often signal cell lysis or tissue damage during permeabilization.
4. **Hemoglobin Content (`pct_counts_hb`)**: In vascularized tissues, red blood cell contamination can skew normalization.

## Sample Python Pipeline (using Scanpy & Squidpy)

Here is a typical starting snippet for spatial data preprocessing:

```python
import scanpy as sc
import squidpy as sq

# Load 10x Visium dataset
adata = sq.read.visium("path/to/outs/")

# Calculate QC metrics
adata.var["mt"] = adata.var_names.str.startswith("MT-")
sc.pp.calculate_qc_metrics(adata, qc_vars=["mt"], inplace=True)

# Spatially aware filtering
sc.pp.filter_cells(adata, min_genes=100)
adata = adata[adata.obs["pct_counts_mt"] < 20].copy()
```

## Moving Forward

Effective quality control is not about eliminating noise at the expense of biology—it is about contextualizing variance across the tissue coordinate space. In future posts, I will dive into spatial clustering benchmarks and graph neural network approaches for cell-cell interaction modeling.


### Resources
+ <https://www.ncbi.nlm.nih.gov/gene/854465>