---
layout: research
title: "Machine Learning Models for Protein–Ligand Binding Affinity Prediction"
date: 2025-12-15 12:00:00 +0530
type: "Research Internship"
venue: "Indian Institute of Technology, Delhi (IIT Delhi)"
authors: "Priyal Tripathi"
tags: [structural-bioinformatics, catboost, xgboost, raspd, iit-delhi]
description: "Curated protein-ligand structural datasets from RCSB PDB to reproduce and benchmark CatBoost and XGBoost models for binding affinity prediction within the RASPD+ framework."
---

## Project Context

Undergraduate Researcher at the **Indian Institute of Technology, Delhi (IIT Delhi)** (June 2025 – December 2025).

Predicting binding affinity between candidate small-molecule ligands and macromolecular target proteins is a critical step in modern computer-aided drug design (CADD). High-fidelity molecular docking simulations and free-energy calculations are computationally expensive; machine learning models that can rapidly and accurately estimate binding constants from structural representations offer significant acceleration.

## Scope & Methodology

1. **Dataset Curation**:
   - Curated high-resolution protein–ligand complex datasets from the **RCSB Protein Data Bank (PDB)**.
   - Verified structural quality, resolution thresholds, and binding assay metrics ($K_i, K_d, IC_{50}$).

2. **Feature Engineering**:
   - Extracted spatial physicochemical features matching the architecture utilized in the **RASPD+** (Rapid Screening with Physicochemical Descriptors) framework.

3. **Gradient Boosting Benchmarks**:
   - Implemented and tuned **CatBoost** and **XGBoost** regressors.
   - Evaluated predictive performance using Root Mean Squared Error (RMSE) and coefficient of determination ($R^2$) across stratified test splits.
