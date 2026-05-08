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

**2. Data Integration**
- Extracted 5 features per trial: `contrast_left`, `contrast_right`, `spks_mean`, `spks_sd`, `total_neurons`
- Applied K-means clustering (k=6, selected via elbow method) to identify shared patterns across sessions
- Performed PCA for dimensionality reduction and cluster visualization

**3. Predictive Model**
- Logistic regression (`glm`) trained on a 75/25 train-test split
- Features: `contrast_right`, `spks_mean`, `spks_sd`, and `spks_mean × spks_sd` interaction term
- Evaluated on internal test split and two held-out external test sets

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
