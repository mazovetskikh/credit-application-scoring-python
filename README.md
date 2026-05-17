# Credit Application Scoring

[![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat&logo=jupyter&logoColor=white)](https://jupyter.org/)

## Overview

A Python-based scoring model that evaluates credit applications using a weighted, rule-based approach. The model processes raw application data, applies business logic across six criteria, and outputs a weekly summary of accepted applications.

This type of scoring pipeline is commonly used in banking and fintech to automate initial application screening.

## Dataset

Two input files:

| File | Description |
|---|---|
| `applications.csv` | Applicant records: age, location, marital status, external credit rating, loan amount, application date |
| `industries.csv` | Industry risk scores (0–20 points) used to weight applicants by sector |

13,278 raw records across 7 weeks (December 2022 – January 2023).

## What the Notebook Does

**1. Data cleaning**
- Removes duplicate `applicant_id` entries
- Fills missing `External Rating` values with 0
- Fills missing `Education level` values with a default category

**2. Pre-scoring filter**

Applications are excluded before scoring if:
- `Amount` is missing or zero
- `External Rating` equals 0 (no credit history on record)

**3. Score calculation**

Each accepted application is scored 0–100 based on six criteria:

| Criterion | Points |
|---|---|
| Age between 35 and 55 | +20 |
| Application submitted on a weekday | +20 |
| Applicant is married | +20 |
| Location: Kyiv or Kyiv region | +10 |
| Industry risk score (from `industries.csv`) | 0–20 |
| External Rating ≥ 7 | +20 |
| External Rating ≤ 2 | −20 |

**4. Output**

Applications with `total_score > 0` are retained. Results are grouped by week, with mean score per week rounded to the nearest integer.

| Week | Avg Score |
|---|---|
| 2022-12-04 | 51 |
| 2022-12-11 | 49 |
| 2022-12-18 | 50 |
| 2022-12-25 | 47 |
| 2023-01-01 | 51 |
| 2023-01-08 | 51 |
| 2023-01-15 | 52 |

## Key Techniques

- Boolean masking with vectorised arithmetic for score calculation
- `left merge` to join industry scores without dropping unmatched applicants
- `resample('W')` for time-based aggregation
- `.clip()` to enforce score boundaries
[![View Notebook](https://img.shields.io/badge/View%20Notebook-F37626?style=flat&logo=jupyter&logoColor=white)](https://github.com/mazovetskikh/credit-application-scoring-python/blob/main/Credit_Application_Scoring.ipynb)
## Files

```
credit-application-scoring/
├── Credit_Application_Scoring.ipynb
├── applications.csv
├── industries.csv
└── README.md
```
