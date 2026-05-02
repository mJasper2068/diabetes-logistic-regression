# Diabetes Prediction (Logistic Regression)

## Overview

This project builds a diabetes prediction model using the Kaggle Pima Indians Diabetes Database.
The goal is to predict whether a patient is likely to have diabetes based on diagnostic health measurements.

Because this is a healthcare-related classification problem, missing a diabetic patient is more critical than sending extra patients for manual review.
For that reason, recall and false negatives are treated as more important than raw accuracy.

---

## Objective

Predict whether a patient is:

- Non-diabetic (0)
- Diabetic (1)

And adjust the decision threshold to reduce missed diabetic cases.

---

## Dataset

The dataset used is the Pima Indians Diabetes Database from Kaggle.

Target column:

- `Outcome`

Features include:

- Pregnancies
- Glucose
- BloodPressure
- SkinThickness
- Insulin
- BMI
- DiabetesPedigreeFunction
- Age

---

## Approach

- Loaded the diabetes dataset
- Separated features `X` and target `y`
- Checked for missing values
- Split the data into train/test sets
- Trained a Logistic Regression model
- Tuned the regularization strength using multiple `C` values
- Evaluated using accuracy and confusion matrix
- Applied threshold tuning to reduce false negatives

---

## Model

Logistic Regression was used as the baseline classification model.

The following `C` values were tested:

- 0.01
- 0.1
- 1
- 10
- 100
- 1000

Test performance reached `0.729` at:

- C = 10
- C = 100
- C = 1000

`C = 10` was selected because it achieved the same test performance as higher values while keeping stronger regularization than `C = 100` or `C = 1000`.
This helps reduce unnecessary model flexibility and lowers overfitting risk.

---

## Threshold Tuning (Key Insight)

The default Logistic Regression threshold is `0.50`, but this may not be the best decision rule for a healthcare-related problem.

A lower threshold was selected:

```text
Threshold = 0.10
```

At this threshold, the model produced:

```text
False Negatives = 2
False Positives = 83
```

This tradeoff is acceptable because missing a patient who may have diabetes is more serious than flagging extra patients for manual review.

---

## Business / Healthcare Application

This model can be used as a screening support tool.

Example use case:

- Predict diabetes probability for each patient
- Flag patients above the selected threshold
- Send flagged patients for further review or testing

The model is not used as a final medical diagnosis. Instead, it supports prioritization and early detection.

---

## Decision Rule

Instead of only predicting diabetes using the default threshold, the model is used to guide action:

> Patients with predicted diabetes probability >= 0.10 should be flagged for further review.

This converts the model output into a practical screening strategy.

---

## Sample Output

Example model decision format:

| Patient ID | Diabetes Probability | Decision |
|---|---:|---|
| 001 | 0.76 | Review |
| 002 | 0.34 | Review |
| 003 | 0.04 | No Review |

Patients above the `0.10` threshold are flagged for further review.

---

## Files

```text
README.md
diabetes_logistic_regression.ipynb
```

---

## Tools Used

- Python
- Pandas
- NumPy
- Scikit-learn
- Jupyter Notebook

---

## Key Learning

The main learning from this project is that model accuracy alone is not enough.

For healthcare-related classification, the cost of a false negative is higher than the cost of a false positive. Threshold tuning allows the model to better match the real-world decision problem.
