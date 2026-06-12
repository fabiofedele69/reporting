# Synthetic AML Risk Modeling and Detection Using Machine Learning

## Capstone Proposal

**Author:** Fabio Fedele

---

## Project Overview

This repository contains the capstone proposal notebook and supporting datasets for the project:

> **Synthetic AML Risk Modeling and Detection Using Machine Learning**

The objective of the project is to investigate how Machine Learning can support Anti-Money Laundering (AML) investigations through:

1. **False Positive Reduction**

   * Anomaly Detection using Isolation Forest
   * Identification of unusual AML cases
   * Investigation prioritization

2. **SAR Filing Prediction**

   * Logistic Regression
   * Gradient Boosting
   * Risk-based prioritization of AML cases

---

## Research Questions

### Research Question 1

Can anomaly detection techniques identify unusual AML cases that deserve greater investigative attention?

Model:

* Isolation Forest

### Research Question 2

Can supervised learning predict whether an investigated AML case is likely to result in a Suspicious Activity Report (SAR) filing?

Models:

* Logistic Regression
* Gradient Boosting

---

## Repository Structure

```text
.
├── README.md
├── requirements.txt
├── c5_project-proposal-Fabio-Fedele-reviewed.ipynb
│
└── data/
    ├── single_tables/
    │   ├── alerts.csv
    │   ├── approvals.csv
    │   ├── case_events.csv
    │   ├── case_narratives.csv
    │   ├── cases.csv
    │   ├── clients.csv
    │   ├── evidence.csv
    │   ├── investigators.csv
    │   ├── reviewers.csv
    │   └── transactions.csv
    │
    └── final_datasets/
        ├── full_dataset.csv
        └── model_dataset.csv
```

---

## Dataset Overview

The project uses a synthetic AML dataset generated through a dedicated AML data generation framework.

The dataset represents the complete AML investigation lifecycle:

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

Two consolidated datasets are provided:

### Full Dataset

Contains:

* Alert information
* Case characteristics
* Investigator attributes
* Governance information
* Final outcomes

Used for:

* Exploratory Data Analysis (EDA)
* Feature inventory and interpretation
* Data lineage analysis
* Case-level walkthrough

Current size:

| Metric   | Value |
| -------- | ----: |
| Cases    | 8,448 |
| Features |    47 |

---

### Modeling Dataset

Machine learning-ready dataset derived from the Full Dataset.

Used for:

* Isolation Forest
* Logistic Regression
* Gradient Boosting
* Model evaluation

Current size:

| Metric   | Value |
| -------- | ----: |
| Cases    | 8,448 |
| Features |    36 |

---

## Outcome Distribution

| Outcome         | Cases | Percentage |
| --------------- | ----: | ---------: |
| CLOSE_NO_ACTION | 5,898 |     69.82% |
| MONITOR         | 1,543 |     18.26% |
| ESCALATE        |   677 |      8.01% |
| SAR_FILED       |   330 |      3.91% |

This distribution reflects a realistic AML environment characterized by class imbalance and rare-event prediction challenges.

---

## Running the Notebook

### Prerequisites

Python 3.10+ recommended.

Install dependencies:

```bash
pip install -r requirements.txt
```

or

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

---

### Launch Jupyter Notebook

```bash
jupyter notebook
```

Open:

```text
c5_project-proposal-Fabio-Fedele-reviewed.ipynb
```

and execute the notebook from top to bottom.

---

## Notebook Contents

The notebook contains:

1. Problem definition and research questions
2. Dataset overview
3. Dataset documentation
4. Full Dataset vs Modeling Dataset comparison
5. Feature inventory and dataset composition
6. Preliminary exploratory data analysis (EDA)
7. Data lineage analysis
8. Case-level construction example
9. Assessment of machine learning feasibility

---

## Notes

The synthetic AML data generation framework itself is not part of the capstone proposal submission.

This repository focuses on the generated datasets and their analysis in support of the proposed machine learning tasks.

---

## Author

**Fabio Fedele**

EPFL Extension School

Applied Data Science & Machine Learning Program
