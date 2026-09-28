---
layout: research
title: "XGB-BIF: An XGBoost-Driven Biomarker Identification Framework for Detecting Cancer Using Human Genomic Data"
date: 2025-06-15 12:00:00 +0530
type: "Journal Article"
venue: "International Journal of Molecular Sciences (IJMS), Vol. 26, Issue 12"
authors: "Veena Ghuriani, Jyotsna Talreja Wassan*, Priyal Tripathi, Anshika Chauhan"
doi: "10.3390/ijms26125590"
paper_url: "https://doi.org/10.3390/ijms26125590"
tags: [machine-learning, xgboost, biomarker-discovery, cancer-genomics, transcriptomics]
description: "A novel machine learning framework leveraging XGBoost feature ranking to discover robust genomic biomarkers across human cancer cohorts, attaining >90% classification accuracy."
---

## Overview

High-throughput transcriptomic sequencing generates tens of thousands of gene expression features per sample, presenting a classic "curse of dimensionality" challenge (*p* >> *n*). In this peer-reviewed publication in the *International Journal of Molecular Sciences (IJMS)*, we introduce **XGB-BIF** (XGBoost-Driven Biomarker Identification Framework), an interpretable machine learning pipeline designed to isolate minimal, highly discriminative gene subsets for cancer detection.

## Key Highlights & Contributions

- **Boosting-Driven Feature Ranking**: Evaluated feature importance metrics (gain, coverage, and frequency) across iterative gradient boosting trees to prioritize stable gene sets.
- **Dimensionality Reduction & High Accuracy**: Compressed large-scale RNA-sequencing matrices down to candidate biomarker panels while exceeding 90% diagnostic accuracy across validation cohorts.
- **Biological Validation**: Mapped prioritized biomarkers to biological signaling pathways using gene ontology (GO) and KEGG pathway enrichment, confirming biological relevance to tumorigenesis and oncogenic regulation.

## Citation

```bibtex
@article{ghuriani2025xgbbif,
  title={XGB-BIF: An XGBoost-Driven Biomarker Identification Framework for Detecting Cancer Using Human Genomic Data},
  author={Ghuriani, Veena and Wassan, Jyotsna Talreja and Tripathi, Priyal and Chauhan, Anshika},
  journal={International Journal of Molecular Sciences},
  volume={26},
  number={12},
  pages={5590},
  year={2025},
  publisher={MDPI},
  doi={10.3390/ijms26125590}
}
```
