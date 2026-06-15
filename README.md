# Synthetic AML Risk Modeling and Detection Using Machine Learning

## Capstone Proposal

**Author:** Fabio Fedele

**Program:** Applied Data Science & Machine Learning

**Institution:** EPFL Extension School

---

# Project Overview

This repository contains the capstone proposal and supporting material for the project:

> **Synthetic AML Risk Modeling and Detection Using Machine Learning**

The project investigates how Machine Learning can support Anti-Money Laundering (AML) investigations by:

* Identifying unusual cases through anomaly detection
* Reducing the impact of false positives
* Predicting Suspicious Activity Report (SAR) filing outcomes
* Supporting risk-based prioritization of investigative resources

The project evaluates two complementary machine learning tasks:

### Task 1 – False Positive Reduction

Research Question:

> Can anomaly detection techniques identify unusual AML cases that deserve greater investigative attention?

Model:

* Isolation Forest

### Task 2 – SAR Filing Prediction

Research Question:

> Can supervised learning predict whether an investigated case is likely to result in a SAR filing?

Models:

* Logistic Regression
* Random Forest

The second task compares an interpretable linear model (Logistic Regression) with a non-linear ensemble model (Random Forest).

---

# Repository Structure

```text
.
├── README.ipynb
├── c5_project-proposal-Fabio-Fedele.ipynb
├── Dataset Origin and Preparation.ipynb
├── Isolation Forest.ipynb
├── Logistic Regression.ipynb
├── Random Forest.md
│
└── data/
    ├── single_tables/
    └── final_datasets/
```

---

# Notebook Guide

## 1. Capstone Proposal

**Notebook**

```text
c5_project-proposal-Fabio-Fedele.ipynb
```

Purpose:

* Project motivation
* Problem definition
* Research questions
* Modeling dataset overview
* Machine learning methodology
* Evaluation strategy
* References to supporting notebooks

This notebook serves as the primary capstone proposal document.

---

## 2. Dataset Origin and Preparation

**Notebook**

```text
Dataset Origin and Preparation.ipynb
```

Purpose:

* Dataset origin
* Source table descriptions
* Construction of the Full Dataset
* Transformation from Full Dataset to Modeling Dataset
* Feature preparation
* Removal of leakage variables
* Data lineage analysis
* Case-level construction examples

This notebook documents the complete data preparation process used to create the machine learning dataset.

---

## 3. Isolation Forest

**Notebook**

```text
Isolation Forest.ipynb
```

Purpose:

* False positive reduction
* Anomaly detection methodology
* Isolation Forest design
* Anomaly scoring approach
* Evaluation strategy

This notebook describes the unsupervised learning approach used to identify unusual AML cases.

---

## 4. Logistic Regression

**Notebook**

```text
Logistic Regression.ipynb
```

Purpose:

* SAR filing prediction
* Binary classification framework
* Logistic Regression methodology
* Regularization strategy
* Evaluation metrics
* Model interpretability

This notebook describes the baseline supervised learning model.

---

## 5. Random Forest

**Notebook**

```text
Random Forest.ipynb
```

Purpose:

* SAR filing prediction
* Tree-based ensemble learning
* Feature importance analysis
* Comparison with Logistic Regression
* Performance evaluation

This notebook describes the comparison model used for supervised learning.

---

# Dataset Overview

The project uses a synthetic AML dataset generated through a dedicated AML data generation framework.

The dataset represents the AML investigation lifecycle:

```text
Client
↓
Transactions
↓
Alerts
↓
Cases
↓
Investigation
↓
Governance Review
↓
Final Outcome
```

---

## Full Dataset

The Full Dataset represents a consolidated case-level analytical view.

It contains:

* Alert information
* Scenario information
* Case characteristics
* Investigator attributes
* Governance information
* Final outcomes

Current size:

| Metric   | Value |
| -------- | ----: |
| Cases    | 8,448 |
| Features |    47 |

The Full Dataset is used for:

* Data exploration
* Feature interpretation
* Data lineage analysis
* Business understanding

---

## Modeling Dataset

The Modeling Dataset is derived from the Full Dataset through a structured preparation process.

The transformation removes:

* Technical identifiers
* Leakage variables
* Information not available during real investigations

Current size:

| Metric   | Value |
| -------- | ----: |
| Cases    | 8,448 |
| Features |    36 |

The Modeling Dataset is used for:

* Isolation Forest
* Logistic Regression
* Random Forest

---

# Outcome Distribution

| Outcome         | Cases | Percentage |
| --------------- | ----: | ---------: |
| CLOSE NO ACTION | 5,898 |     69.82% |
| MONITOR         | 1,543 |     18.26% |
| ESCALATE        |   677 |      8.01% |
| SAR FILED       |   330 |      3.91% |

This distribution reflects realistic AML challenges including class imbalance and rare-event prediction.

---

# Notes

The synthetic AML data generation framework itself is not part of this submission.

The repository focuses on:

* Dataset documentation
* Dataset preparation
* Exploratory analysis
* Machine learning methodology
* Model evaluation strategy

All datasets required by the notebooks are included within the repository.

---

# Author

**Fabio Fedele**

EPFL Extension School

Applied Data Science & Machine Learning Program
