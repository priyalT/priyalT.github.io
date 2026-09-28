---
layout: post
title: "Computational Quality Control in Spatial Transcriptomics"
date: 2026-09-28 10:00:00 +0530
description: "A practical breakdown of spot filtering, mitochondrial thresholds, and artifact detection in 10x Visium datasets."
tags: [spatial-omics, bioinformatics, python]
---

Spatial transcriptomics has revolutionized how we map gene expression within the intact morphological context of tissue sections. However, unlike dissociated single-cell RNA-sequencing (scRNA-seq), spatial assays introduce distinct technical artifacts—ranging from tissue folding and permeabilization leakage to uneven sequencing depth across slide coordinates.

## Why Standard scRNA-seq QC Falls Short

In standard scRNA-seq workflows, filtering spots solely by total counts (UMIs) and detected genes (features) is standard practice. In spatial transcriptomics, however, cell density naturally varies across tissue architecture:

> Highly dense tumor cores will naturally exhibit higher UMI counts than hypocellular stroma or necrotic zones. Blindly applying uniform global cutoffs risks erasing biologically meaningful low-density regions.

### Key Metrics to Evaluate:

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
