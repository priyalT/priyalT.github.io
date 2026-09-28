---
layout: post
title: "How would bulk rna-seq look like with barely 20GB of remaining storage?"
date: 2026-09-28
description: "An easy-to-follow tutorial on Bulk RNA-seq"
tags: [rna-seq, bioinformatics, python]
published: false
---

Bulk RNA-seq analysis is one of the most fundamental methods in transcriptomics. It is essentially used to quantify gene expression between different conditions. Studying transcriptomics could have multiple results, like transcriptome assembly, refinement of gene models, metatranscriptomics, differential gene expression analysis, etc. In this tutorial, we will walk through bulk RNA-seq analysis from raw reads to gene set enrichment analysis, all in less than 20 GB.
As a student, one of the most key obstacles in my bioinformatics analyses has been storage. I work with an 8 GB Macbook Air M1 (ancient, I know), and I can understand when an experiment with a lot of potential falls short because of computational cost. To ensure that this doesn't end up discouraging new learners in the field, I have made this guide extremely efficient for different kinds of specs.


## Transcriptomics

RNA, or ribonucleic acid, is key in gene expression. A transcript is an RNA molecule, produced by the process of transcription (DNA --> RNA). In a broad sense, RNAs can be of two types: coding RNA and non-coding RNA, based on whether the RNA has protein-coding potential. mRNA is the major type of protein-coding RNA, and serves as a template for translation to produce proteins, which is a major step in gene expression. Non-coding RNAs generally do not serve as templates for protein synthesis, but perform a variety of functions, including acting as catalysts, adaptor molecules, etc. Examples of non-coding RNA are siRNA, miRNA, tRNA, rRNA, and more.


### What will we be working on?

Within this tutorial, we will be performing differential gene expression analysis. Our samples belong to a study performed on *Saccharomyces cerevisiae* where half of the samples lack the snf2 gene. The snf2 gene is responsible for multiple functions, especially including ATP-dependent chromatin remodeler activity. It is also a part of the SWI/SNF complex, and contributes to DNA binding activity, DNA metabolic processes, and regulation of gene expression. Through this project, we maintain the key biological question: 
> Which genes in yeast are essentially dependent on remodelling done by snf2, and how much are they affected by the absence of snf2?

## Curating our data

We start by collecting the raw fastq files from [project: PRJEB5348](https://www.ebi.ac.uk/ena/browser/view/PRJEB5348?show=reads). The original dataset has 48 biological and 7 technical replicates of two conditions: wild type vs. snf2 knockout mutant RNA-seq of *S. cerevisiae*. A biological replicate is an independently obtained biological sample from the same experimental condition, whereas a technical replicate is a repeated measurement of the same biological sample or sequencing library, used to assess technical variability. For this experiment, I used 6 biological replicates of both conditions (12 samples in total). I utilised the mapping provided on the project's website and obtained the following samples using the following script:


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

```bash
wget -nc ftp URL of fastq files
```

If you're using MacOS, you can still use wget by installing it with Homebrew. wget is preferred over curl for many reasons, one of them being that wget can handle download retries even over unreliable connections. 
> Read more: <https://daniel.haxx.se/docs/curl-vs-wget.html>

### Understanding the FASTQ format
Before we move on, let us try understanding the files we have obtained. A fastq file has a recognizable format that is also easy to understand:

```
@ERR458495.1 DHKW5DQ1:219:D0PT7ACXX:3:1101:2236:2048/1
CAGTTGTGACCAGATGGAGTCATTCTACCGTCCTTGACGGCCCAGACTTCT
+
CCCFFFFFHHHHHJJJJJJGIIJJJJJJJIHIIJJJJJIJIJJJJJIJIJH
```
- The first line starts with '@' followed by the identifier of the sample and further description
- The second line contains the actual sequence of the read
- '+' character takes up the third line
- Line four includes quality scores that are Phred-encoded.

> Decoding phred quality scores is easy! First, determine the ASCII decimal value of the quality-score character and subtract 33 from it (for Phred+33 encoding). For example, the ASCII decimal value of 'C' is 67, so the Phred quality score is: 67 − 33 = 34. A Phred score of 30 corresponds to an error probability of 1 in 1,000, implying that the accuracy would be estimated to be 99.9%. Higher Phred scores indicate lower probabilities of error.


### FastQC
FastQC is a quality control tool for sequence data. We will run it for our raw sequenced reads by running the following code in the directory where our raw data is present.
```
fastqc
```



### Resources
+ <https://www.ncbi.nlm.nih.gov/gene/854465>