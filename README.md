# Neuroscientific Predictive Model using Session-Based Datasets

A statistical machine learning project that predicts trial outcomes from mouse neural spike data using logistic regression, K-means clustering, and PCA.

STA 141A — UC Davis | Instructor: Prof. SZ Chen | Author: Peter Zhang

---

## Overview

This project uses a subset of data from Steinmetz et al. (2019), consisting of 18 experimental sessions where visual stimuli were presented to four mice. The goal is to predict trial feedback (success or failure) based on neural activity in the visual cortex and stimulus contrast conditions.

---

## Dataset

- **18 sessions** across 4 mice: Cori, Forssmann, Hench, Lederberg
- Each trial contains:
  - `feedback_type` — outcome: 1 (success) or -1 (failure)
  - `contrast_left` / `contrast_right` — visual stimulus contrast levels
  - `spks` — neuron spike counts in the visual cortex across time bins
  - `brain_area` — brain region of each recorded neuron

---

## Methodology

**1. Exploratory Data Analysis**
- Summary table across sessions: neuron counts, trial counts, brain areas, success rates
- Failure rate trends across sessions
- Cumulative feedback across trials to assess learning behavior
- Homogeneity/heterogeneity analysis of trial outcomes and contrast conditions across mice

**2. Data Integration & Clustering**
- Extracted 5 features per trial: `contrast_left`, `contrast_right`, `spks_mean`, `spks_sd`, `total_neurons`
- Features standardized via z-score scaling before clustering
- Applied **K-means clustering** (k=6, selected via elbow method on within-group sum of squares across k=1–15) to group trials with shared neural and stimulus patterns across sessions
- Applied **PCA** on the scaled feature matrix; first two principal components used for 2D cluster visualization via `fviz_cluster`

**3. Predictive Model**
- Target variable: `feedback_type` (1 = success, -1 = failure)
- Model: **Logistic regression** fit with `glm(..., family = "gaussian")` on a stratified 75/25 train-test split using `sample.split()`
- Features: `contrast_right`, `spks_mean`, `spks_sd`, and a `spks_mean × spks_sd` interaction term to capture joint effects of spike rate and variability
- Decision threshold set at 0.6 (predict -1 if probability > 0.6, else 1)
- Evaluated via confusion matrix and misclassification error rate on both an internal test split and two held-out external test sets

---

## Results

| Evaluation | Misclassification Rate |
|------------|----------------------|
| Internal test set | 34.1% |
| External test sets | 28.5% |

The model performs well on predicting successes (class 1) but struggles with failures (class -1), likely due to class imbalance in the training data.

---

## Stack

- **Language:** R
- **Libraries:** `tidyverse`, `ggplot2`, `factoextra`, `cluster`, `caTools`, `knitr`
- **Output:** R Markdown → HTML report

---

## Files

| File | Description |
|------|-------------|
| `STA PROJECT copy .Rmd` | Full R Markdown source: EDA, clustering, modeling, evaluation |
| `STA-PROJECT-copy-.html` | Rendered HTML report with all plots and results |
