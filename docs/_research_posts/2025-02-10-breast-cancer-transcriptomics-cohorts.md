---
layout: research
title: "Exploring Breast Cancer Transcriptomics Across Global and Indian Cohorts: A Comparative Study"
date: 2025-02-10
type: "Research Internship"
venue: "Indian Institute of Technology, Roorkee (IIT Roorkee)"
authors: "Priyal Tripathi (Supervisor: Dr. Aparajita Khan)"
tags: [breast-cancer, transcriptomics, deg-analysis, umap, iit-roorkee]
description: "Comparative transcriptomic analysis across TCGA (n=815) and Indian cohorts (n=81), identifying CFH, DST, and COPZ2 as candidate universal biomarkers across subtypes."
---

## Project Overview

Undergraduate Research Internship conducted at the **Indian Institute of Technology, Roorkee (IIT Roorkee)** under the supervision of **Dr. Aparajita Khan**.

Most public cancer transcriptomic datasets represent predominantly Western demographic populations. In this study, we evaluated whether gene expression landscapes and candidate diagnostic biomarkers translate consistently to Indian breast cancer cohorts.

## Methodology & Findings

1. **Cohort Integration & Preprocessing**:
   - Analyzed global TCGA breast cancer RNA-seq data (*n* = 815) alongside an Indian patient cohort (*n* = 81).
   - Applied batch-correction and normalization pipelines to ensure cross-cohort comparability.

2. **Unsupervised Clustering & Differential Expression (DEA)**:
   - Evaluated transcriptional heterogeneity using **UMAP** dimensionality reduction.
   - Performed differential expression analysis across clinically defined molecular subtypes (Estrogen Receptor `ER`, Progesterone Receptor `PR`, and `HER2`).
   ![UMAP Indian Cluster](/assets/images/research/umap_indian.png)
   ![UMAP Global Cluster](/assets/images/research/umap_global.png)

3. **Candidate Universal Biomarkers**:
   - Identified 3 shared DEGs—**`CFH`**, **`DST`**, and **`COPZ2`**—consistently dysregulated across both global and Indian populations regardless of subtype heterogeneity.
   - These genes represent potential pan-cohort prognostic candidates warranting further clinical validation.
   ![UMAP CFH](/assets/images/research/CFH.png)
   ![UMAP COPZ2](/assets/images/research/COPZ2.png)


4. **Indian Cohort Specific Biomarkers**:
   - Genes such as KIT and SFRP1, which were mostly present in the Indian dataset, could indicate regional specificity, highlighting the value in incorporating diverse groups of people in genomic analysis
   ![UMAP KIT](/assets/images/research/KIT.png)
   ![UMAP SFRP1](/assets/images/research/SFRP1.png)
   - Longitudinal samples from Indian patients might be used to confirm the prognostic utility of identified biomarkers
