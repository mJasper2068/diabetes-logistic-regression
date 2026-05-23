# Diabetes Prediction (Logistic Regression)

## Overview

This project builds a diabetes prediction model using the Pima Indians Diabetes Database from Kaggle.

The goal is to predict whether a patient is likely to have diabetes based on diagnostic health measurements. Because this is a healthcare-related classification problem, missing a diabetic patient is more critical than flagging extra patients for manual review. Recall and false negatives are treated as more important than raw accuracy.

---

## Dataset

Pima Indians Diabetes Database from Kaggle.

```
https://www.kaggle.com/datasets/uciml/pima-indians-diabetes-database
```

The dataset contains 768 patient records. Target column: `Outcome` (1 = Diabetic, 0 = Non-diabetic).

**Features:** Pregnancies, Glucose, BloodPressure, SkinThickness, Insulin, BMI, DiabetesPedigreeFunction, Age

---

## Approach

This project was built without a sklearn Pipeline to demonstrate manual preprocessing steps. Later projects in this portfolio use Pipeline for production-ready structure.

- Loaded and inspected the dataset using `df.describe()`, `df.info()`, and `df.head()`
- Checked for missing values
- Split the data into train/test sets with `stratify=y`
- Tuned regularization strength using multiple C values
- Applied `class_weight='balanced'` to handle class imbalance
- Applied threshold tuning to minimize false negatives

---

## Model

Logistic Regression with `class_weight='balanced'` to account for the imbalanced target distribution (500 non-diabetic vs 268 diabetic).

**C Tuning Results:**

| C | Train Score | Test Score |
|---|---|---|
| 0.01 | 0.792 | 0.729 |
| 0.1 | 0.792 | 0.734 |
| 1 | 0.792 | 0.729 |
| 10 | 0.793 | 0.729 |
| 100 | 0.793 | 0.729 |

**Selected: C = 0.1**

C=0.1 achieved the earliest best test score of 0.734. When scores are tied, the lower C is preferred as it applies stronger regularization and reduces overfitting risk.

---

## Threshold Tuning

The default threshold of 0.50 was lowered to minimize false negatives. Accuracy was not used as the primary evaluation metric as it decreases when the threshold is lowered, which does not reflect the true goal of minimizing missed diabetic cases.

| Threshold | FN | FP |
|---|---|---|
| 0.50 (base) | 20 | 29 |
| 0.40 | 10 | 40 |
| 0.30 | 5 | 53 |
| 0.20 | 1 | 75 |
| 0.10 (final) | 0 | 104 |

**Final threshold: 0.10**

At 0.10, FN was reduced from 20 to 0. The increase in FP from 29 to 104 is acceptable because missing a diabetic patient carries a higher cost than routing additional patients for manual review by healthcare staff.

---

## Healthcare Application

This model functions as a screening support tool, not a final diagnosis.

- Predict diabetes probability for each patient
- Flag patients above the 0.10 threshold for further review
- Route flagged patients to clinical testing

---

## Limitations

- Small dataset (768 records) may limit generalizability
- Zero values in Glucose, BloodPressure, Insulin, and BMI likely represent missing data rather than true zero readings, not addressed in this version
- Model is not suitable as a standalone diagnostic tool

---

## Future Improvements

- Replace zero values with imputed estimates using median or domain-informed values
- Add sklearn Pipeline for cleaner preprocessing structure
- Test additional models (Random Forest, XGBoost) for comparison

---

## Technologies Used

- Python
- Pandas
- NumPy
- Scikit-learn
- Jupyter Notebook

---

## Repository Structure

```
├── diabetes_logistic_regression.ipynb
├── README.md
└── diabetes.csv
```
