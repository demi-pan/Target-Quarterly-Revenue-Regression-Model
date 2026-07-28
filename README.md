# Target Corp. Quarterly Revenue Regression Model

A time-series regression model that predicts Target Corporation's (NYSE: TGT) quarterly revenue using a linear trend and seasonal dummy variables for the Q3 (back-to-school) and Q4 (holiday) demand periods.

Overview
This project builds an OLS regression model to forecast Target's quarterly net sales (saleq) from 2000–2023, using quarterly financial data sourced from a Compustat-style dataset (qSales_2024.csv). The model captures Target's long-term revenue growth trend and recurring seasonal spikes tied to the back-to-school and holiday shopping seasons.

Data
Source file: qSales_2024.csv
Coverage: Quarterly financial data for multiple public companies (2000–2023); this analysis filters to Target Corp. (tic == 'TGT')
Key fields used:
Column	Description
datadate	Fiscal quarter end date
fqtr	Fiscal quarter (1–4)
saleq	Quarterly revenue (dependent variable)
Methodology
Data cleaning — Filtered the raw dataset to Target Corp. records and converted datadate to a proper datetime type.
Feature engineering:
time: sequential time index (independent variable capturing the linear growth trend)
christmas_DV: binary dummy equal to 1 for Q4 (holiday season)
christmas_DV_interaction: time × christmas_DV, allowing the Q4 seasonal effect to change over time
backtoschool_DV: binary dummy equal to 1 for Q3 (back-to-school season)
backtoschool_DV_interaction: time × backtoschool_DV
Train/test split — First 75% of observations used for training; remaining 25% (2018 Q1–2023 Q1) held out for testing.
Model — Ordinary Least Squares (OLS) regression via statsmodels, regressing saleq on time, the two seasonal dummies, and their interaction terms.
Evaluation — Mean Absolute Percentage Error (MAPE) computed on the held-out test set.
Results
Metric	Value
Test MAPE	~12.0%
The fitted coefficients show a positive linear trend (~$136M in incremental quarterly revenue per period) plus a strong Q4 seasonal lift (~$4,105M above baseline), consistent with Target's holiday-driven sales pattern. Predicted values track the overall trend and seasonal cycle of actual revenue reasonably well, though the model tends to underpredict the magnitude of the most recent Q4 peaks.

Repository Structure
├── qSales_2024.csv           # Raw quarterly financial dataset
├── target_regression.ipynb   # Analysis notebook (data prep, model, evaluation)
└── README.md
How to Run
Install dependencies:
bash
   pip install pandas numpy matplotlib statsmodels
Place qSales_2024.csv in the working directory.
Run the notebook cells in order: data load → filter/clean → feature engineering → train/test split → model fit → evaluation → visualization.
Limitations
Single-company, univariate time trend model — does not incorporate macroeconomic variables (e.g., consumer spending indices, inflation) that could improve accuracy.
Fixed 75/25 chronological split rather than rolling/expanding-window validation.
MAPE understates error sensitivity during high-revenue Q4 periods, where absolute dollar misses are largest.
Next Steps
Incorporate macroeconomic covariates (GDP growth, consumer confidence, inflation).
Test a log-linear specification to address heteroscedasticity in revenue growth.
Compare against alternative forecasting approaches (e.g., SARIMA, panel regression across peer retailers).

