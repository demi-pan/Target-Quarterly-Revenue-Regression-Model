# Target Corp. Quarterly Revenue Regression Model

A time-series regression model that predicts Target Corporation's (NYSE: TGT) quarterly revenue using a linear trend and seasonal dummy variables for the Q3 (back-to-school) and Q4 (holiday) demand periods.

## Overview

This project builds an OLS regression model to forecast Target's quarterly net sales (`saleq`) from 2000–2023, using quarterly financial data sourced from a Compustat-style dataset (`qSales_2024.csv`). The model captures Target's long-term revenue growth trend and recurring seasonal spikes tied to the back-to-school and holiday shopping seasons.

## Data

- **Source file:** `qSales_2024.csv`
- **Coverage:** Quarterly financial data for multiple public companies (2000–2023); this analysis filters to Target Corp. (`tic == 'TGT'`)
- **Key fields used:**

| Column | Description |
|---|---|
| `datadate` | Fiscal quarter end date |
| `fqtr` | Fiscal quarter (1–4) |
| `saleq` | Quarterly revenue (dependent variable) |

## Methodology

1. **Data cleaning** — Filtered the raw dataset to Target Corp. records and converted `datadate` to a proper datetime type.
2. **Feature engineering:**
   - `time`: sequential time index (independent variable capturing the linear growth trend)
   - `christmas_DV`: binary dummy equal to 1 for Q4 (holiday season)
   - `christmas_DV_interaction`: `time × christmas_DV`, allowing the Q4 seasonal effect to change over time
   - `backtoschool_DV`: binary dummy equal to 1 for Q3 (back-to-school season)
   - `backtoschool_DV_interaction`: `time × backtoschool_DV`
3. **Train/test split** — First 75% of observations used for training; remaining 25% (2018 Q1–2023 Q1) held out for testing.
4. **Model** — Ordinary Least Squares (OLS) regression via `statsmodels`, regressing `saleq` on `time`, the two
