# Bank Risk Connectedness Under Financial Stress

This project analyzes systemic risk and interbank connectedness among major U.S. banks under financial stress. Using daily equity return data from nine major financial institutions, the project examines whether market stress is associated with stronger return comovement and higher systemic risk.

## Project Overview

The analysis focuses on the relationship between financial stress and interbank connectedness. Financial stress is measured using market stress indicators such as the VIX and the St. Louis Fed Financial Stress Index (STLFSI). Interbank connectedness is measured through average pairwise correlations among bank stock returns.

The project includes exploratory data analysis, stress vs. calm period comparison, rolling correlation analysis, regression modeling, model diagnostics, and out-of-sample validation.

## Research Question

How does financial stress affect connectedness among major U.S. banks?

More specifically, this project investigates whether banks become more correlated during high-stress market periods, increasing the potential for systemic risk transmission.

## Data

The analysis uses daily financial data for nine major U.S. financial institutions:

- AIG
- BAC
- C
- GS
- JPM
- MS
- PNC
- USB
- WFC

Market stress and control variables include:

- VIX
- STLFSI
- S&P 500 market return proxy

The sample period covers January 2010 to March 2020.

## Methodology

The project applies the following methods:

- Daily log return calculation
- Stress vs. calm period comparison
- Average pairwise correlation as a systemic risk proxy
- 252-day rolling interbank correlation
- OLS regression
- HAC robust standard errors
- Lagged dependent variable model
- ANOVA-based model comparison
- Expanding-window out-of-sample RMSE validation

## Key Findings

- Bank return correlations are higher during stress periods than calm periods.
- Average interbank correlation increases substantially under elevated market stress.
- Rolling correlation analysis shows that connectedness changes over time and rises during turbulent periods.
- Regression diagnostics indicate heteroskedasticity and serial correlation, so HAC standard errors are used for more reliable inference.
- The lagged correlation model achieves the lowest out-of-sample RMSE, suggesting that interbank connectedness is highly persistent over time.

## Repository Structure

```text
bank-risk-connectedness-financial-stress/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── data/
│   └── bank_returns_and_financial_stress_daily.xlsx
│
├── notebooks/
│   └── risk_connectedness_analysis.ipynb
│
├── report/
│   └── systemic_risk_connectedness_report.pdf
│
└── figures/
    ├── jpm_returns_stress_vs_calm.png
    ├── avg_bank_returns_vs_vix.png
    ├── rolling_interbank_correlation.png
    ├── stress_connectedness_3d.png
    └── rmse_model_comparison.png
