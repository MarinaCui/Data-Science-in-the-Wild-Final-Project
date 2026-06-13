# Socioeconomic Gradient in U.S. Health Outcomes

A data science study of how **state-level median household income** relates to
**chronic-disease mortality** (heart disease and diabetes) across the United States —
and how much of that relationship runs through **health behaviors** such as obesity,
smoking, and physical activity.

> Final project for **CS 5304 — Data Science in the Wild**, Cornell Tech (Spring 2026).

---

## Overview

This project investigates whether a **socioeconomic gradient** exists in U.S. health
outcomes and, if so, how much of it is explained by behavioral factors versus income
acting as an independent predictor. Using a cross-sectional dataset of all 50 states
plus D.C. for 2024, we combine public data sources, run preregistered hypothesis tests,
and fit regression models to quantify these relationships.

## Research Questions

- **Primary:** To what extent does state-level median household income explain variation
  in chronic-disease mortality (heart disease and diabetes) across U.S. states in 2024?
- **Secondary:** How much of the income–mortality relationship is mediated by health
  behaviors (obesity, smoking, physical inactivity), versus income acting as an
  independent predictor?

## Data

|                       |                                                                          |
| --------------------- | ------------------------------------------------------------------------ |
| **Unit of analysis**  | U.S. state (50 states + D.C.)                                            |
| **Sample size**       | 51 observations                                                          |
| **Design**            | Cross-sectional (2024)                                                   |
| **Sources**           | U.S. Census Bureau (ACS 2024 1-Year Estimates); Kaiser Family Foundation (KFF) State Health Facts 2024 |

**Key variables**

- **Independent (socioeconomic):** `Median_Income`
- **Outcomes:** `Heart_Disease`, `Diabetes` (deaths per 100,000)
- **Behavioral / controls:** `Obesity`, `Smoking`, `Physical_Activity`,
  `Diabetes_Prevalence`, `Poor_Health`, `Uninsured_Adults`

Raw sources were cleaned (metadata removal, reshaping the income file from wide to long,
type coercion), merged on state name via an inner join, and mean-imputed for a small
number of suppressed values. The final dataset — `final_state_health_dataset_2024.csv` —
contains **51 rows × 10 variables** with no missing values.

## Methods

- Preregistered hypotheses (stated before analysis)
- Exploratory data analysis (distributions, correlations, scatterplots)
- Ordinary least squares (OLS) regression — bivariate and multivariate
- Log-transformation of income to address right skew
- Methodological checks: mediation vs. confounding, multicollinearity (condition number),
  and residual diagnostics (Jarque–Bera, Omnibus)

## Key Findings

| Hypothesis                                                | Model                                   | Result                     | R²              | p-value |
| --------------------------------------------------------- | --------------------------------------- | -------------------------- | --------------- | ------- |
| H1 — Higher income → lower heart-disease mortality        | `Heart_Disease ~ log(Income)`           | Supported (β = −158.4)     | 0.63            | < 0.001 |
| H2 — Higher income → lower diabetes mortality             | `Diabetes ~ log(Income)`                | Supported (β = −19.0)      | 0.46            | < 0.001 |
| H3 — Higher income → more physical activity               | `Physical_Activity ~ log(Income)`       | Supported (β = +0.17)      | 0.55            | < 0.001 |
| H4 — Income predicts mortality after behavioral controls  | `Heart_Disease ~ log(Income) + controls`| Supported (β = −121.4)     | 0.68 (adj 0.65) | 0.0019  |

**Takeaways**

- Income is a strong, statistically significant predictor of chronic-disease mortality
  across states.
- About **77%** of the bivariate income–mortality association persists after controlling
  for obesity, smoking, physical activity, and the uninsured rate — evidence that income
  operates **both directly and indirectly** (through behaviors).
- Concretely, a **10% higher median income** corresponds to roughly **12 fewer
  heart-disease deaths per 100,000**, all else equal.
- Improving population health likely requires addressing **underlying socioeconomic
  disparities**, not behavioral interventions alone.

## Repository Structure

```
.
├── Phase_2.ipynb                        # Data collection & cleaning pipeline
├── Phase_4.ipynb                        # EDA, hypothesis testing & regression
├── final_state_health_dataset_2024.csv  # Cleaned, merged analysis dataset
└── README.md
```

## Reproducing the Analysis

```bash
# 1. Clone the repository
git clone https://github.com/MarinaCui/Data-Science-in-the-Wild-Final-Project.git
cd Data-Science-in-the-Wild-Final-Project

# 2. Install dependencies
pip install pandas numpy statsmodels matplotlib seaborn jupyter

# 3. Launch the notebooks
jupyter notebook Phase_4.ipynb
```

## Limitations

- **Small sample (n = 51):** limits statistical power and makes estimates sensitive to
  outliers (e.g., Deep South states).
- **Ecological fallacy:** all variables are state-level aggregates; relationships may not
  hold at the individual level.
- **Multicollinearity:** income and behavioral variables overlap, shifting attribution
  among correlated predictors.
- **Cross-sectional design:** 2024 only — cannot establish causality or temporal dynamics.

## Team

- Hongyiming (Marina) Cui
- Xiaohui Zang
- Jiawei Wang
- George Zhu

---

*Built with Python · pandas · statsmodels · matplotlib · seaborn*
