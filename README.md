# lab_07-23MID0381-predictive-analysis

# Recommendation System from Customer Transaction Data using Random Forest

![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.6.1-orange.svg)
![pandas](https://img.shields.io/badge/pandas-2.2.2-green.svg)
![Status](https://img.shields.io/badge/Status-Completed-success.svg)

> **Advanced Predictive Analytics (Lab 07)**
> An end-to-end, reproducible, and leakage-safe machine learning workflow for constructing a personalized Top-K recommendation system using transaction history, candidate generation, and a supervised Random Forest ranking pipeline.

---

## Table of Contents

1. [Overview](#overview)
2. [Datasets](#datasets)
3. [Project Workflow](#project-workflow)
4. [Models Evaluated](#models-evaluated)
5. [Key Results](#key-results)
6. [Repository Structure](#repository-structure)
7. [How to Run](#how-to-run)
8. [License](#license)

---

## Overview

This repository contains the implementation and technical reporting for predicting future customer purchases based on implicit feedback (transaction history). The project reframes recommendation as a supervised ranking problem, utilizing a Random Forest classifier to score customer-item candidates.

Special emphasis is placed on temporal data integrity (chronological splitting), leakage-safe feature engineering (RFM and interaction signals), reproducible negative sampling, and retrieval-stage diagnostics (Candidate-Recall auditing).

---

## Datasets

The project utilizes a verified e-commerce transactional dataset. Strict data integrity rules were enforced to remove cancellations and missing customer identifiers.

### UCI Online Retail Dataset

- **Raw Samples:** 541,909 transactions
- **Filtered Samples:** 392,732 (post-cancellation and missing ID scrubbing)
- **Scope:** 4,339 unique customers and 3,665 unique items spanning one year (Dec 2010 – Dec 2011)
- **Target:** Future purchase/interaction within a designated temporal window (binary implicit feedback)

---

## Project Workflow

The project follows an industry-standard recommendation and candidate-ranking pipeline:

1. Established a reproducible environment using a fixed random seed (`SEED = 42`).
2. Performed a strict chronological split (70% Train, 15% Validation, 15% Locked Test) to eliminate future-to-past data leakage.
3. Defined a bounded Candidate Universe comprising the top 1,000 most popular items to optimize computational overhead.
4. Executed a Candidate-Recall Audit to measure retrieval-stage limits (achieving approximately 67% recall at the candidate level).
5. Implemented reproducible negative sampling (1:8 positive-to-negative ratio) to construct balanced user-item classification pairs.
6. Engineered 15 leakage-safe features (Customer RFM, Item popularity, User-Item historical interactions).
7. Trained and tuned a Random Forest Classifier via validation ranking performance.
8. Generated dynamic Top-K personalized recommendations based on predicted purchase probabilities.
9. Evaluated final rankings using `Recall@K` and `NDCG@K` against baseline models.
10. Conducted error analysis on sparse cold-start users and generated feature importance visualizations.

---

## Models Evaluated

The following ranking methodologies were implemented and benchmarked under identical evaluation conditions:

- **Popularity Baseline:** Non-personalized top-purchased items
- **Item-Item Collaborative Filtering:** Advanced native recommendation benchmark via Cosine Similarity
- **Random Forest Classifier:** Supervised candidate scoring utilizing RFM and interaction features

---

## Key Results

The **Random Forest model** dramatically outperformed both generic baselines by successfully leveraging complex, non-linear interactions among historical user-item interactions and RFM features.

### Final Holdout Test Performance (Top-10 Recommendations)

| Model | Recall@10 | NDCG@10 | Description |
| :-------------------- | :-------: | :-----: | :---------- |
| **Popularity** | 0.0349 | 0.0861 | Generic global bestseller baseline |
| **Item-Item CF** | 0.1182 | 0.2328 | Interaction-based matrix heuristic |
| **Random Forest** | **0.3121** | **0.5997** | Supervised interaction and feature scorer |

### Major Findings

- **Superior Ranking:** The Random Forest model achieved a superior NDCG@10 of 0.5997, significantly outperforming the Item-Item CF and Popularity baselines.
- **Feature Importance:** Gini importance diagnostics revealed that historical direct engagement (`pair_spend`, `pair_qty`, `pair_purchases`) and item recency (`item_recency_days`) were the strongest predictors of future relevance.
- **Cold-Start Vulnerability:** While the RF model excelled for users with rich histories, qualitative error analysis revealed vulnerability when serving sparse/cold-start users (e.g., users with only 1 past transaction), indicating a need for fallback strategies.
- **Retrieval Limits:** Bounding the candidate catalog to 1,000 items successfully constrained the scoring space but capped the maximum possible recall at approximately 67%.

---

## Repository Structure

```text
House-Price-Prediction/
├── 23MID0381_Lab07_Recommender_RF.ipynb     # Complete recommendation system workflow
├── 23MID0381_Lab07_Report.pdf               # Detailed lab report and interpretations
├── models/                                  # Serialized models (.joblib)
├── artifacts/                               # JSON schemas (dataset card, split manifest, feature cols)
├── exports/                                 # Exported metrics (Ranking metrics, Error Analysis, Candidate Recall)
├── figures/                                 # Generated plots (Feature Importance, Precision-Recall curves)
└── README.md                                # Project documentation
How to Run
Prerequisites

The project was developed and tested using the following software versions:

Software	Version
Python	3.10+
pandas	2.2.2
scikit-learn	1.6.1
numpy	1.26+

Install the required dependencies using:

pip install pandas scikit-learn numpy matplotlib seaborn joblib openpyxl
Running the Project
Clone or download this repository.
Ensure the Online Retail.xlsx or Online Retail.csv dataset is placed in the root directory. The script includes an auto-upload prompt for Google Colab users if the file is missing.
Open the notebook:
23MID0381_Lab07_Recommender_RF.ipynb

using Jupyter Notebook, JupyterLab, or Google Colab.

Execute Restart Kernel and Run All Cells (or simply Run All) to reproduce the complete workflow.

The notebook will automatically perform:

Transaction audit and cleaning
Chronological data splitting
Feature engineering and dataset construction
Model training, validation, and advanced CF benchmarking
Metric extraction
Visualization generation
.zip bundle export

No additional configuration is required. The environment seed is locked to ensure deterministic outputs.

License

This project was developed as part of the Advanced Predictive Analytics (Lab 07) coursework and is intended for educational purposes.
