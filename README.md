# Bank Risk Connectedness and Financial Stress Analysis

## Project Overview

This project examines how major U.S. bank equity returns move together under different financial stress conditions. Using daily return data for nine major financial institutions alongside market stress indicators, the analysis investigates whether interbank return correlations are systematically higher during periods of market stress — a pattern with direct implications for systemic risk monitoring and portfolio diversification.

The work covers descriptive analysis, stress vs. calm period comparisons, rolling interbank correlation, regression modeling with robust standard errors, ANOVA-based model selection, and expanding-window out-of-sample validation.

---

## Research Question

Does financial stress increase return connectedness among major U.S. banks, and if so, how do stress indicators such as VIX relate to the level and persistence of interbank correlation?

---

## Motivation

When banks become highly correlated during stress periods, standard diversification assumptions break down and losses can propagate rapidly across the system. Understanding the relationship between financial stress indicators and interbank connectedness is relevant for:

- Systemic risk monitoring
- Portfolio diversification assessments under different market regimes
- Financial stability analysis

---

## Data

**File:** `data/raw/bank_returns_and_financial_stress_daily.xlsx`

The dataset contains daily observations for nine major U.S. financial institutions and associated market stress variables. The sample period covers January 2010 to March 2020.

**Banks included:**

| Ticker | Institution |
|--------|------------|
| AIG | American International Group |
| BAC | Bank of America |
| C | Citigroup |
| GS | Goldman Sachs |
| JPM | JPMorgan Chase |
| MS | Morgan Stanley |
| PNC | PNC Financial |
| USB | U.S. Bancorp |
| WFC | Wells Fargo |

**Stress and control variables:**

- `VIXCLS` — CBOE Volatility Index (VIX)
- `STLFSI` — St. Louis Fed Financial Stress Index
- `stress` — Binary stress indicator (derived from VIX)
- `MKT` — Broad market return proxy

---

## Methodology

The analysis proceeds in the following steps:

1. **Descriptive analysis** — Summary statistics and annualized volatility for all banks across the full sample, stress periods, and calm periods.
2. **Stress vs. calm comparison** — Distributions of individual bank returns (e.g., JPM) are compared between stress and calm regimes.
3. **Average pairwise correlation** — A systemic risk proxy is computed as the mean pairwise correlation across all bank pairs.
4. **Rolling interbank correlation** — A 252-day rolling window tracks how average pairwise correlations evolve over the sample period.
5. **Regression modeling** — OLS models regress average interbank correlation on stress indicators (VIX, STLFSI), a binary stress flag, and a lagged correlation term. HAC/Newey-West robust standard errors are used throughout to address heteroskedasticity and serial correlation.
6. **Model comparison** — Three model specifications are compared: a baseline (stress indicators only), an interaction model (stress × VIX), and a lag model (adding a lagged dependent variable). ANOVA F-tests and expanding-window RMSE validation are used to compare models.
7. **GARCH extension** — A simple GARCH(1,1) model is fitted to average interbank correlation to assess whether its variance is time-varying.

> All regression results are diagnostic and descriptive. This analysis does not claim causal identification.

---

## Repository Structure

```
bank-risk-connectedness-financial-stress/
│
├── README.md
├── LICENSE
├── .gitignore
├── requirements.txt
│
├── data/
│   └── raw/
│       └── bank_returns_and_financial_stress_daily.xlsx
│
├── figures/
│   ├── exploratory/
│   │   ├── avg_bank_returns_vs_vix.png
│   │   └── jpm_returns_stress_vs_calm.png
│   ├── connectedness/
│   │   ├── rolling_interbank_correlation.png
│   │   └── stress_connectedness_3d.png
│   └── model_diagnostics/
│       └── rmse_model_comparison.png
│
├── notebooks/
│   ├── bank_risk_connectedness_analysis.ipynb
│   └── capstone_project_notebook_export.pdf
│
└── reports/
    └── capstone_project_report.pdf
```

---

## Key Visualizations

### Average Bank Returns vs. VIX
`figures/exploratory/avg_bank_returns_vs_vix.png`

Scatter plot of the equal-weighted average daily bank return against VIX. Illustrates the negative relationship between market volatility and bank return levels.

### JPM Returns: Stress vs. Calm Periods
`figures/exploratory/jpm_returns_stress_vs_calm.png`

Distribution of JPMorgan daily returns separated by stress regime. Shows the difference in tail behavior and dispersion across regimes.

### Rolling Interbank Correlation
`figures/connectedness/rolling_interbank_correlation.png`

252-day rolling average pairwise correlation across all nine banks. Captures how systemic connectedness evolves over time and rises during turbulent market periods.

### Stress Connectedness (3D)
`figures/connectedness/stress_connectedness_3d.png`

3D visualization of time, VIX, and rolling average correlation, colored by VIX level. Provides an overview of the joint dynamics of stress and connectedness over the sample period.

### RMSE Model Comparison
`figures/model_diagnostics/rmse_model_comparison.png`

Out-of-sample RMSE across expanding-window cross-validation folds for three model specifications. Used to compare predictive fit rather than in-sample goodness of fit.

---

## Main Findings

- Bank return co-movement appears stronger during stress periods than during calm periods, as measured by average pairwise correlation.
- Rolling interbank correlation increases during turbulent market episodes, suggesting that systemic connectedness is time-varying rather than fixed.
- Stress indicators (VIX, STLFSI) are associated with higher average interbank correlation. The interaction between the stress regime and VIX is statistically significant.
- Regression diagnostics indicate heteroskedasticity and serial correlation in OLS residuals; HAC robust standard errors are used throughout for more reliable inference.
- The lag model — which includes a one-period lagged correlation — achieves the lowest out-of-sample RMSE across cross-validation folds, consistent with strong persistence in interbank connectedness.
- Financial stress variables retain statistically significant coefficients even after controlling for persistence (lag model), suggesting incremental explanatory power beyond serial correlation alone.

> These findings are descriptive. Correlation and connectedness measures do not establish causal relationships.

---

## How to Reproduce

### Prerequisites

```bash
pip install -r requirements.txt
```

### Run the notebook

```bash
jupyter notebook notebooks/bank_risk_connectedness_analysis.ipynb
```

The notebook is designed to run from the repository root. The data file is loaded with a relative path (`../data/raw/bank_returns_and_financial_stress_daily.xlsx`), so it should work without any path changes as long as the directory structure is preserved.

The notebook runs end-to-end with the exception of the optional GARCH extension cells, which require the `arch` package. Install it separately if needed:

```bash
pip install arch
```

---

## Requirements

See [requirements.txt](requirements.txt) for the full list. Core dependencies:

| Package | Purpose |
|---------|---------|
| pandas | Data loading and manipulation |
| numpy | Numerical operations |
| matplotlib | Plotting |
| seaborn | Statistical visualizations |
| scipy | Statistical tests (Shapiro-Wilk, t-test, ANOVA) |
| statsmodels | OLS regression, HAC errors, VAR, ANOVA |
| scikit-learn | RMSE calculation |
| openpyxl | Reading `.xlsx` data file |

---

## Limitations

- The binary stress indicator is derived from VIX levels. Results may vary with alternative stress definitions.
- Correlation and connectedness are descriptive measures and do not establish causal relationships between stress and bank co-movement.
- Equity returns are an indirect and noisy measure of bank risk. They reflect market perceptions rather than fundamental balance sheet exposures.
- The nine banks in this sample represent large U.S. financial institutions. Results may not generalize to smaller banks, non-U.S. institutions, or other time periods.
- This analysis is not a trading strategy, a regulatory stress test, or a predictive forecasting system.
- The RMSE validation results should be interpreted as diagnostic comparisons between model specifications, not as evidence of real-world predictive accuracy.

---

## Portfolio Notes

This project was completed as a quantitative capstone in financial econometrics. It demonstrates applied skills in:

- Time-series data analysis with financial market data
- Regression diagnostics and robust inference (HAC standard errors)
- Rolling-window analysis and regime-based comparisons
- Model selection using out-of-sample validation (expanding-window RMSE)
- Statistical testing (F-tests, ANOVA, Breusch-Pagan, Durbin-Watson, Jarque-Bera)
- Financial data visualization

The full report is available in [reports/capstone_project_report.pdf](reports/capstone_project_report.pdf).

---

## License

[MIT License](LICENSE)
