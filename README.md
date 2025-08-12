# CPSM_BRCA
Overview

This repository contains a comprehensive survival analysis of The Cancer Genome Atlas (TCGA) breast cancer cohort using the Cancer Patient Survival Model (CPSM) R package. We analyzed 1,081 breast cancer patients to identify key genetic biomarkers associated with survival outcomes and developed predictive models achieving excellent performance (C-index: 0.78).
Key Findings

Identified 1,440 genes significantly associated with survival (p < 0.05)

PSME2: Strongest protective factor (HR: 0.061, 94% risk reduction)

UGP2: Highest risk factor (HR: 9.324) - novel finding in breast cancer

Combined clinical-genetic model achieves C-index of 0.78

Methodology
Data Processing

Source: TCGA-BRCA via TCGAbiolinks

Samples: 1,081 primary tumors with complete survival data

Features: 5,000 most variable genes + clinical variables

Normalization: Log2(FPKM + 1) transformation

Statistical Analysis

Univariate Cox Regression: All genes tested for survival association

Feature Selection: Top 50 genes by p-value (p < 0.05)

Model Development: Multi-Task Logistic Regression (MTLR) via CPSM

Model 1: Clinical features only

Model 2: Gene expression only

Model 3: Combined clinical + genetic

Performance Metrics

C-index: 0.78 (excellent discrimination)

Calibration: Well-calibrated across risk groups

Validation: 80/20 train-test split


<img width="4200" height="3000" alt="brca_survival_dashboard" src="https://github.com/user-attachments/assets/9b508dcc-d7d6-40a5-8a70-7623f02b3911" />

