# Sex Differences in Rates of Hearing Decline Before and After Menopause in Koreans

**Authors:**  
**Study design:** Cross-sectional  
**Journal submission:** In revision

---

## Overview

This repository contains the full statistical re-analysis pipeline for the manuscript:

> *"Sex Differences in Rates of Hearing Decline before and after Menopause in Koreans: A Cross-sectional Study"*

The study examines whether the rate of age-associated hearing decline differs between men and women, with a specific focus on whether menopause (breakpoint: 49.3 years, Korean population mean) accelerates hearing loss in women.

---

## Repository Contents

| File | Description |
|---|---|
| `statistical_analysis.Rmd` | Full analysis pipeline in R Markdown — open in RStudio and knit to HTML or PDF |
| `statistical_analysis.ipynb` | Same pipeline in Python (Jupyter Notebook) |
| `README.md` | This file |

> **Data files (`men.xlsx`, `women.xlsx`) are not included** in this repository as they contain participant-level audiological data. Contact the corresponding author for data access.

---

## Data

- **Men:** n = 910, age range 18–79 years
- **Women:** n = 3,175, age range 18–79 years
- **Frequencies measured:** 0.25, 0.5, 1, 2, 4, 8 kHz (pure-tone audiometry)
- **Breakpoint:** 49.3 years (established Korean mean menopause age; Park et al., 2017)

---

## Analysis Pipeline

The analysis is organised into 6 phases:

### Phase 1 — Data Loading & Cleaning
Loads both datasets, validates column structure, removes missing values, and merges into a single dataframe.

### Phase 2 — Exploratory Data Analysis (EDA)
- Age distribution by sex
- Audiogram: mean hearing threshold by age decade (replicates Fig 2)
- Threshold distributions at each frequency
- Skewness and kurtosis assessment
- Correlation heatmap (Age vs each frequency)

### Phase 3 — Imbalance Handling: Propensity Score Matching (PSM)
Men (n=910) and women (n=3,175) are unequal in size (~1:3.5 ratio). PSM creates a 1:1 age-balanced subsample for sensitivity analysis. Primary analysis uses the full unmatched sample.

### Phase 4 — Regression Assumption Checking
- Normality of residuals (Shapiro-Wilk / Kolmogorov-Smirnov)
- Homoscedasticity (Breusch-Pagan test)
- Residual vs. fitted plots
- Outlier detection (Cook's Distance)

### Phase 5 — Violation Remedies
- **Weighted Least Squares (WLS):** inverse-variance weights by 5-year age bin
- **HC3 Robust Standard Errors:** sandwich covariance for heteroscedasticity
- **Bootstrap Percentile 95% CI:** 2,000 resamples, no normality assumption
- **FDR Correction:** Benjamini-Hochberg across all 12 hypothesis tests

### Phase 6 — Model Building & Visualisation
- Piecewise linear regression at fixed breakpoint 49.3 yr (replicates Fig 3)
- Rate-of-decline bar chart with bootstrap CI (replicates Fig 4)
- OLS vs WLS vs Robust slope comparison table
- Sex-difference interaction test (Age × Sex) with FDR correction
- Sensitivity analysis: breakpoints at 47.3, 48.3, 49.3, 50.3, 51.3 yr
- PSM sensitivity: ANCOVA repeated on age-matched sample

---

## Statistical Methods Summary

| Issue | Method applied |
|---|---|
| Sex-group imbalance (1:3.5) | Propensity Score Matching (sensitivity) |
| Non-normality of residuals | Bootstrap CIs, robust SEs |
| Heteroscedasticity | WLS, HC3 robust standard errors |
| Multiple comparisons (12 tests) | Benjamini-Hochberg FDR correction |
| Non-linear age–hearing relationship | Piecewise linear regression (breakpoint = 49.3 yr) |
| Sex difference in rate of decline | Linear regression with Age × Sex interaction term |

---

## How to Run

### R Markdown (`statistical_analysis.Rmd`)

**Requirements:** R ≥ 4.0, RStudio

Install required packages (run once in RStudio console):

```r
install.packages(c("tidyverse", "readxl", "MatchIt", "sandwich",
                   "lmtest", "boot", "scales", "patchwork", "writexl"))
```

Then open `statistical_analysis.Rmd` in RStudio and click **Knit → Knit to HTML** or **Knit to PDF**.

> Place `men.xlsx` and `women.xlsx` in the same folder as the `.Rmd` file before knitting.

### Jupyter Notebook (`statistical_analysis.ipynb`)

**Requirements:** Python ≥ 3.8, Jupyter Lab or Jupyter Notebook

Install required packages:

```bash
pip install pandas numpy scipy statsmodels scikit-learn matplotlib seaborn openpyxl
```

Then run:

```bash
jupyter lab
```

---

## Key Findings

- Rates of hearing decline (dB/year) estimated via piecewise linear regression with a fixed breakpoint at 49.3 years
- Results are compared to the Baltimore Longitudinal Study of Aging (Pearson et al., 1995) to validate cross-sectional slope estimates against established longitudinal data
- Sex differences in rates of decline tested using linear regression with Age × Sex interaction term, FDR-corrected across all frequencies

---

## Reference

Pearson JD, Morrell CH, Gordon-Salant S, et al. Gender differences in a longitudinal study of age-associated hearing loss. *J Acoust Soc Am.* 1995;97(2):1196–205.

Park CY, Lim J-Y, Park H-Y. Age at natural menopause in Koreans: secular trends and influences thereon. *Menopause.* 2017;25(4):423–9.
