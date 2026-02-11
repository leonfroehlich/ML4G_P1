# Prediction of Gene Expression from Chromatin Landscape

Machine Learning for Genomics (Fall 2025) - Project 1

## Project Overview
This project focuses on predicting gene expression levels from chromatin landscape data using machine learning models.

Gene expression is regulated by multiple epigenetic mechanisms such as histone modifications and chromatin accessibility. The objective of this project is to train a predictive model on known cell lines and evaluate its ability to generalize to an unseen cell line.

## Problem Statement
Given chromatin features from three human cell lines (X1, X2, X3) aligned to hg38 (CRCh38):
- Train a model on:
  - Chromosome set A of X1 and X2
- Validate on:
  - Chromosome set B of X1 and X2
- Test on:
  - Chromosome set C of X3 (helt-out)

The evaluation metric is Spearman's rank correlation (ρ) between predicted gene expression and true CAGE expression on the test set. Baseline requirement: ρ ≥ 0.685

## Biological Background
Gene expression is strongly influenced by chromatin state. The following epigenetic datasets were provided:

Histone Modifications (ChIP-seq)
- H3K27me3
- H3K4me1
- H3K4me3
- H3K27ac
- H3K36me3
- H3K9me3

Chromatin Accessibility
- DNase-seq

Target
- Gene expression (CAGE)
- Gene annotation (TSS, gene body, strand)

## Methodology
### Feature Engineering
To construct biologically meaningful features:
- Extracted signal intensity from BigWig files
- Aggregated signals around:
  - Promoter region (+/- 2kb around TSS)
  - Extended regulatory regions (up to 200kb where relevant)
- Applied normalization to ensure cross-cell-line comparability
- Generated per-gene feature vectors combining:
  - Histone marks
  - Chromatin accessibility

Design decisions were guided by biolgoical evidence suggesting:
- ~47% regulatory information is withing 40kb of TSS
- ~84% within 200kb

### Model Selection
Multiple models were explored:
- Linear Regression
- Ridge / Lasso Regression
- Random Forest Regressor
- Gradient Boosting

Model selection was based on validation performance (Spearman's correlation).

Final chose model:
**Random Forest Regressor**

## Training Strategy
- Training: Chromosomes in set A (X1,X2)
- Validation: Chromosomes in set B (X1,X2)
- Test: Chromosome 1 (X3)

Cross-validation was performed within A+B to prevent overfitting.

## Evaluation Metrics

Primary metric:

**Spearman’s Correlation (ρ)**

Measures rank consistency between predicted and true gene expression:

$\rho = 1 - \frac{6\sum d_i^2}{n(n^2 - 1)}$

Why Spearman?
- Expression levels may not be linearly related to chromatin signal
- Ranking of genes (high vs low expression) is biologically meaningful
- Robust to monotonic transformations

Additional metrics evaluated:
- Pearson correlation
- R² score
