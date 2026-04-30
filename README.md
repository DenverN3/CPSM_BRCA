# CPSM_GBM — Glioblastoma Survival Analysis

MTLR-based survival modeling for TCGA glioblastoma multiforme (GBM) using the [CPSM](https://bioconductor.org/packages/CPSM/) Bioconductor package.

## Overview

This repository applies the Cancer Patient Survival Model (CPSM) pipeline to TCGA-GBM, identifying prognostic genes and building predictive survival models using Multi-Task Logistic Regression (MTLR).

**Key features:**
- Overall Survival (OS) endpoint with ~82% event rate — the highest in the pan-cancer panel
- GBM molecular subtype (Proneural/Classical/Mesenchymal/Neural) as the primary clinical feature (AJCC staging is not applicable to GBM)
- Five MTLR model architectures comparing clinical, genomic, and combined predictors
- 867 univariate survival-significant genes identified (p<0.05)
- Clinical features (Age + molecular subtype) outperform gene-based models on test data

## Pipeline

| Step | Function | Description |
|------|----------|-------------|
| 1 | Data acquisition | TCGA-GBM via TCGAbiolinks |
| 2 | `data_process_f()` | Survival time conversion (days → months) |
| 3 | `tr_test_f()` | 80/20 train-test split |
| 4 | `train_test_normalization_f()` | Log2 + quantile normalisation |
| 5a | `Lasso_PI_scores_f()` | LASSO feature selection + PI score |
| 5b | `Univariate_sig_features_f()` | Univariate Cox screening |
| 6 | `MTLR_pred_model_f()` | 5 MTLR models |
| 7–11 | Visualisation | Survival curves, KM overlay, nomogram, risk group prediction |

## Results

| Model | Train C-index | Test C-index |
|-------|:---:|:---:|
| Model 1 — Clinical (Age + Subtype) | 0.60 | 0.71 |
| Model 2 — PI score | 0.88 | 0.59 |
| Model 3 — PI + Clinical | 0.88 | 0.62 |
| Model 4 — Univariate genes | 0.71 | 0.62 |
| Model 5 — Genes + Clinical | 0.73 | 0.70 |

Best model: **Model 1 (Clinical only)** — test C-index 0.71. GBM molecular subtypes capture prognostic information more robustly than individual gene expression features, consistent with the established role of Verhaak et al. subtype classification in GBM biology.

## GBM-Specific Notes

- **No AJCC staging.** GBM is uniformly WHO grade IV; standard cancer staging does not apply. The `gbm_subtype` feature (from `paper_Subtype` annotations) replaces AJCC staging in all models.
- **Small cohort (~150 patients).** Despite the small sample size, the 82% event rate provides ample statistical power for survival modelling (~123 events).
- **Gene universe validation.** Comparison with an earlier independent run confirmed 95.6% HR direction concordance and 100% significance retention among shared genes.

## Requirements

- R ≥ 4.4
- [CPSM](https://bioconductor.org/packages/CPSM/), TCGAbiolinks, SummarizedExperiment, survival, glmnet, MTLR

## Usage

```r
source("CPSM_TCGA_GBM_Pipeline.R")
```

The script auto-detects cached TCGA data in the working directory and loads from `df.rds` or the GDC cache.

## Data

- **Source:** [TCGA-GBM](https://portal.gdc.cancer.gov/projects/TCGA-GBM) via TCGAbiolinks
- **Samples:** ~150 primary tumour patients (after deduplication)
- **Endpoint:** OS — ~123 events (82%)
- **Genes:** Top 20,000 by variance → 867 univariate significant (p<0.05)

## Part of Pan-Cancer Analysis

This pipeline is one of seven TCGA cohorts (COAD, BRCA, GBM, LUAD, LUSC, PAAD, PRAD) analysed for hypoxia-associated survival genes. Cross-cancer findings include 193 significant hypoxia hits and 6 genes significant in 3+ cancer types.

## References

1. Liu J et al. An Integrated TCGA Pan-Cancer Clinical Data Resource. *Cell*. 2018;173(2):400-416. [doi:10.1016/j.cell.2018.02.052](https://doi.org/10.1016/j.cell.2018.02.052)
2. CPSM: R-package of an Automated Machine Learning Pipeline for Predicting the Survival Probability of Single Cancer Patient. Harpreet Kaur, Pijush Das, Kevin Camphausen, Uma Shankavaram
bioRxiv 2024.11.14.623597; doi: https://doi.org/10.1101/2024.11.14.623597
3. Verhaak RGW et al. Integrated genomic analysis identifies clinically relevant subtypes of glioblastoma. *Cancer Cell*. 2010;17(1):98-110.

## Author

Denver Ncube
