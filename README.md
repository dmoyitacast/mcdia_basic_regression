# What drives a country's GDP per capita? — Multiple linear regression

🌐 **English** · [Español](README.es.md)

> 🎓 **Academic project** · MSc in Data Science and Artificial Intelligence (MCDIA) · Afi Escuela · 2025-26
> Module: *Basic Regression* · Individual assignment

![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white)
![statsmodels](https://img.shields.io/badge/statsmodels-OLS-4B8BBE)
![pandas](https://img.shields.io/badge/pandas-150458?logo=pandas&logoColor=white)

## Goal

Build an **interpretable** linear regression model that explains differences in GDP per capita across 227 countries using demographic, health, geographic and economic-structure indicators, and turn the results into well-reasoned recommendations.

## Data

`data/countries_gdp.csv`: 227 countries × 20 variables (population, area, net migration, infant mortality, literacy, phones per 1,000 people, land use, climate, birth/death rates and the share of agriculture, industry and services in the economy).

## Methodology

| Stage | What is done |
|---|---|
| **Exploratory analysis** | Univariate analysis of every variable, correlation matrix and pairwise scatter plots |
| **Missing values** | Imputation by **regional median** (numeric variables) and **regional mode** (`Climate`); economic sectors and land use renormalised so they sum to 1 |
| **Transformations** | Log transforms of `GDP`, `Infant.mortality` and `Phones` to reduce skewness and linearise relationships |
| **Feature selection** | **Best Subset Selection** comparing adjusted R², AIC and BIC |
| **Diagnostics** | Studentised residuals, leverage, Cook's distance and multicollinearity (VIF) |
| **Model assumptions** | Linearity, zero-mean residuals, homoscedasticity, independence and normality (Shapiro-Wilk, Anderson-Darling, Q-Q plot) |
| **Evaluation** | Comparison of alternative specifications: with and without influential observations, with and without transformations |

## Final model

$$
\log(GDP) = \beta_0 + \beta_1 \log(\text{Infant mortality}) + \beta_2 \log(\text{Phones}) + \beta_3\,\text{Net migration} + \beta_4\,\text{Agriculture} + \beta_5\,\text{Deathrate} + \beta_6\,\text{Crops} + \varepsilon
$$

| Specification | N | Adj. R² | AIC | BIC |
|---|---|---|---|---|
| **Full sample (selected model)** | 226 | **0.824** | 337.5 | 361.4 |
| Without the 3 most influential points | 223 | 0.854 | 284.0 | 307.8 |
| No log transforms on predictors | 226 | 0.813 | 351.2 | 375.1 |

The **full sample** is kept even though removing influential observations improves the metrics: those countries are not measurement errors but real, extreme economies that are part of the phenomenon being studied.

## Key findings

- **Connectivity** (`Phones`) and **positive net migration** are associated with higher GDP per capita.
- **Infant mortality** and an economy **dependent on the primary sector** (agriculture, crops) are associated with lower income.
- The positive sign of `Deathrate` is interpreted as a **demographic composition effect** (older populations in richer countries), not a causal relationship.
- Because the analysis is observational and cross-sectional, policy recommendations are framed as **associations, not causation**.

## Repository structure

```
├── gdp_regression.ipynb   # Full analysis, commented step by step (in Spanish)
├── data/
│   └── countries_gdp.csv  # Dataset
└── requirements.txt
```

## How to run

```bash
python -m venv venv
venv\Scripts\activate          # Windows  (source venv/bin/activate on macOS/Linux)
pip install -r requirements.txt
jupyter notebook gdp_regression.ipynb
```

---

*Completed as part of the assessment for the MSc in Data Science and Artificial Intelligence at Afi Escuela. Grade and assignment brief not included.*
