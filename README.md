## Dementia Risk Prediction using Machine Learning

## Overview

The proposed architecture is a **non-invasive, machine learning–based dementia risk prediction framework** designed to analyze the **combined influence of sociodemographic, lifestyle, cognitive, genetic, and health-related factors**. The model leverages a **Random Forest classifier** supported by a rigorous **feature selection, preprocessing, and optimization pipeline**, ensuring high predictive performance, robustness, and interpretability.

This architecture is specifically tailored for **early-stage dementia detection** using structured clinical data, eliminating reliance on **expensive or invasive diagnostic procedures** such as MRI, PET scans, or cerebrospinal fluid analysis.

## High-Level Architecture Flow

**Raw NACC Dataset → Data Cleaning & Imputation → Feature Selection → Class Balancing → Model Training & Optimization → Dementia Risk Prediction**

Each stage is carefully designed to address real-world clinical data challenges such as **missing values, multicollinearity, class imbalance, and feature redundancy**.

## Dataset Input

* **Source:** National Alzheimer’s Coordinating Center (**NACC UDS dataset**)
* **Initial Dataset Size:** 198,627 records × 1,024 features
* **Population:** Cognitively normal individuals, mild cognitive impairment (MCI), and dementia cases
* **Feature Categories:**

  * **Sociodemographic**
  * **Lifestyle**
  * **Health**
  * **Cognitive assessments (MMSE, MoCA)**
  * **Genetic factors (APOE4 allele presence)**

## Data Preprocessing Layer

### 🔹 Data Cleaning

* Removed features with **>50% missing values**
* Excluded records with **age < 60 years (NACCAGE)**

### 🔹 Missing Value Imputation

* Applied **Multiple Imputation by Chained Equations (MICE)**
* Effectively preserves **complex variable relationships** and reduces bias compared to simple imputation

### 🔹 Variance Filtering

* Eliminated **low-variance features** using Variance Thresholding
* Reduced feature count to **64 informative features**

## Feature Selection & Engineering

### 🔹 Importance-Based Feature Ranking

A combination of complementary feature selection techniques was employed:

* **Random Forest Feature Importance**
* **Recursive Feature Elimination (RFE)**

Feature rankings were cross-validated against **existing dementia literature** to ensure **clinical relevance**.

### 🔹 Multicollinearity Reduction

* Computed **Pearson and Spearman correlation matrices**
* Highly correlated features were removed to reduce **multicollinearity**

### 🔹 Final Feature Set

* **Final number of selected features:** **21**
* Represents the most predictive and clinically meaningful dementia risk factors

## Class Balancing Strategy

* Original training data exhibited **significant class imbalance**
* Applied **SMOTE (Synthetic Minority Oversampling Technique)** on the training set

**Post-SMOTE Class Distribution:**

* Non-Demented (0): 91,321
* Demented (1): 91,321

This step ensures **balanced learning** and prevents model bias toward the majority class.

## Model Core: Random Forest Classifier

### 🌲 Architecture Choice

The **Random Forest** model was selected due to its:

* Ability to handle **mixed data types**
* Robustness to **outliers and noisy clinical data**
* Built-in **feature importance estimation**
* Scalability to **large, high-dimensional datasets**

### 🔹 Training Strategy

* Trained using **k-fold cross-validation** on the training set
* Ensures model generalization and stability

### 🔹 Hyperparameter Optimization

* Employed **Randomized Search Cross-Validation (RandomizedSearchCV)**
* Optimized key parameters such as:

  * Number of trees
  * Maximum tree depth
  * Minimum samples per split

## Model Output & Interpretability

* **Binary classification output:**

  * **0 → Non-Demented**
  * **1 → Demented**

* Feature importance analysis highlights **key contributing factors**, with **INDEPEND (Level of Independence)** emerging as the most influential feature

This interpretability makes the model suitable for **clinical decision support** and **risk stratification**.

## Performance Summary

* **Accuracy:** **92.06%**
* **ROC–AUC Score:** **96.55%**

The high ROC–AUC demonstrates strong discriminatory power between demented and non-demented individuals.

## Architectural Strengths

* **Non-invasive and cost-effective** alternative to imaging-based diagnosis
* **Highly interpretable** compared to black-box deep learning models
* **Scalable** to large longitudinal healthcare datasets
* Robust handling of **missing data, imbalance, and multicollinearity**

## Summary

The proposed architecture integrates **rigorous data preprocessing**, **clinically informed feature selection**, **class imbalance correction**, and a **Random Forest–based predictive core** to deliver a reliable and interpretable dementia risk prediction system. This design enables early detection, supports clinical workflows, and provides a scalable foundation for future extensions using longitudinal or multimodal data.
