# Detecting and Predicting Fraud by Healthcare Providers

This project focuses on developing machine learning models to detect and predict fraudulent activities by healthcare providers within the Medicare domain. By analyzing claims data, the project aims to identify patterns and key factors associated with fraud to safeguard the integrity of healthcare services.

---

## Table of Contents
- [Introduction](#introduction)
- [Objectives](#objectives)
- [Dataset](#dataset)
- [Data Preprocessing](#data-preprocessing)
- [Feature Engineering](#feature-engineering)
- [Modeling and Evaluation](#modeling-and-evaluation)
- [Results](#results)

---

## Introduction

Fraudulent activities in healthcare, such as billing for unprovided services and misrepresentation, result in significant financial losses and compromise patient care. This project analyzes Medicare claims data to identify fraudulent practices and predict high-risk providers. Key research questions include:
1. Identifying factors and behaviors indicative of fraud.
2. Assessing correlations between claim types and fraud frequency.
3. Predicting fraud-prone providers based on location, specialty, and claim volume.

---

## Objectives

1. Detect healthcare providers engaged in fraudulent activities.
2. Predict potentially fraudulent providers using claims data.
3. Uncover key patterns associated with fraud.
4. Develop models for fraud prevention to protect consumers and healthcare providers.

---

## Dataset

The project uses the **Healthcare Provider Fraud Detection Analysis** dataset from Kaggle. The dataset includes:
- **Beneficiary data**: Demographic details and health status.
- **Inpatient and Outpatient claims**: Information on medical services rendered.
- **Target data**: Identifies fraudulent providers.

Combined, the dataset contains 556,728 rows and 54 features.

---

## Data Preprocessing

1. **Cleaning**:
   - Removed duplicates and null rows.
   - Converted categorical variables to binary (e.g., `1/2` to `0/1`).
   - Created new features:
     - `Age` (calculated from `DOB` and `DOD`).
     - `Admit_duration` (difference between admission and discharge dates).
     - `is_dead` (derived from `DOD`).

2. **Merging**:
   - Combined inpatient and outpatient datasets with a `ClaimType` column.
   - Joined with beneficiary and target data using `BeneID` and `Provider` as keys.

---

## Feature Engineering

1. **Correlation Analysis**:
   - Used a correlation matrix for numerical columns.
   - Applied Chi-square and Cramér's V for categorical data.

2. **Feature Selection**:
   - Dropped columns with >60% missing values.
   - Excluded secondary and tertiary diagnosis codes based on domain expertise.

3. **Provider-Based Aggregation**:
   - Transformed data to a provider-centric structure using `groupby`:
     - Summed diagnosis-related features.
     - Aggregated amount-related features by sum and median.

4. **Handling Class Imbalance**:
   - Applied **SMOTE** to balance the target variable (`PotentialFraud`).

---

## Modeling and Evaluation

1. **Models Used**:
   - Logistic Regression
   - Random Forest with hyperparameter tuning
   - XGBoost with RandomizedSearchCV

2. **Best Results**:
   - **XGBoost** achieved:
     - Accuracy: 98%
     - F1-Score for fraud = 1: 91%

3. **Explainability**:
   - Used **SHAP** to interpret feature importance:
     - High reimbursement amounts strongly correlated with fraud.
     - Deductible amounts and regions (e.g., West and South) contributed significantly.

---

## Results

### Key Observations:
- Claims related to obstetric and heart conditions showed higher fraud rates.
- A small subset of physicians accounted for a disproportionately high number of fraud cases.
- Fraudulent cases often involved zero reimbursement amounts.

### Classification Report:
| Metric         | Precision | Recall | F1-Score | Support |
|----------------|-----------|--------|----------|---------|
| Class 0 (No Fraud) | 0.99      | 0.99   | 0.99     | 975     |
| Class 1 (Fraud)    | 0.88      | 0.93   | 0.91     | 106     |
| **Accuracy**        |           |        | **0.98** | 1081    |

---

## License

This repository is licensed under the [MIT License](LICENSE).
