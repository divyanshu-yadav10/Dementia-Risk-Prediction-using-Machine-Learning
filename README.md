# Dementia Risk Prediction using Machine Learning

## Overview

The proposed architecture is a **non-invasive, machine learning-based dementia risk prediction framework** designed to analyze the **combined influence of sociodemographic, lifestyle, cognitive, genetic, and health-related factors**. The model leverages a **Random Forest classifier** supported by a rigorous **feature selection, preprocessing, and optimization pipeline**, ensuring high predictive performance, robustness, and interpretability.

This architecture is specifically tailored for **early-stage dementia detection** using structured clinical data, eliminating reliance on **expensive or invasive diagnostic procedures** such as MRI, PET scans, or cerebrospinal fluid analysis.

## High-Level Architecture Flow

**Raw NACC Dataset -> Target Leakage Removal & Splitting -> Imputation & Scaling -> Feature Selection -> Pipeline-Integrated SMOTE & Model Optimization -> Dementia Risk Prediction**

Each stage is carefully designed to address real-world clinical data challenges such as **missing values, multicollinearity, class imbalance, and feature redundancy**.

## Dataset Input

* **Source:** National Alzheimer's Coordinating Center (NACC UDS dataset)
* **Population:** Cognitively normal individuals, mild cognitive impairment (MCI), and dementia cases. Records with age < 60 years (NACCAGE) are filtered out.
* **Feature Categories:** Sociodemographic, Lifestyle, Health, and Genetic factors (APOE4).

## Critical Architectural Update: Leakage Prevention

To ensure the model generalizes to real-world clinical scenarios and does not "cheat" by using downstream consequences of a dementia diagnosis, strict leakage prevention steps were integrated:

* **Target Leakage Exclusion:** Direct clinical determinants and downstream indicators (e.g., NACCMOCA, INDEPEND, NACCLIVS, RESIDENC) were explicitly removed.
* **Patient-Aware Splitting:** Utilizing StratifiedGroupKFold across Train/Validation/Test splits ensures that no single patient's data appears in more than one partition.

## Data Preprocessing Layer

### Data Cleaning & Imputation

* Removed features with >50% missing values (threshold derived solely from the training set).
* Applied Iterative Imputation (equivalent to Multiple Imputation by Chained Equations - MICE) to preserve complex variable relationships. Imputers are strictly fit on the training data.
* Applied standard scaling to normalize feature distributions.

### Variance & Collinearity Filtering

* Eliminated low-variance features using Variance Thresholding.
* Computed Pearson and Spearman correlation matrices; highly correlated features (>0.85) were removed to reduce multicollinearity.

## Feature Selection

A combination of complementary feature selection techniques was employed:

* **Random Forest Feature Importance**
* **Recursive Feature Elimination (RFE)**

Feature rankings emphasize the most predictive and clinically meaningful dementia risk factors while explicitly excluding previously identified leaky variables like Level of Independence (INDEPEND).

## Class Balancing Strategy

* Original training data exhibited significant class imbalance.
* Applied **SMOTE (Synthetic Minority Oversampling Technique)**.
* **Crucial Update:** SMOTE is now applied dynamically inside a cross-validation pipeline (ImbPipeline) rather than globally on the entire training set. This prevents synthetic data bleed and ensures rigorous, unbiased validation.

## Model Core: Random Forest Classifier

### Architecture Choice

The Random Forest model was selected due to its ability to handle mixed data types, its robustness to outliers, and its built-in feature importance estimation.

### Hyperparameter Optimization

* Employed **Randomized Search Cross-Validation (RandomizedSearchCV)**.
* Optimized parameters across the pipeline (including trees, depth, and split requirements) using patient-grouped k-fold cross-validation.

## Performance Summary

Previous iterations of the model reported an inflated ROC-AUC of 96.55%, which was primarily driven by target leakage (e.g., including independence level features).

Following the strict removal of leaky features and implementing patient-grouped cross-validation, the model achieves a highly robust, clinically realistic performance metric:

* **Test ROC-AUC: 0.7461**

This score reflects the model's true, uninflated discriminatory power when predicting dementia risk from purely foundational demographic, lifestyle, and health data.
