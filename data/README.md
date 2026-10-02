# CreditScope — Data Directory

## Download Instructions

1. Go to the Kaggle competition page: [Give Me Some Credit](https://www.kaggle.com/c/GiveMeSomeCredit/data)
2. Download `cs-training.csv`
3. Place it in `data/raw/` (this directory)

> **Note:** The raw data files are gitignored. Every team member must download their own copy.

---

## Raw Data Immutability Principle

**Raw data in `data/raw/` is NEVER modified in place.**

Every transformation in the pipeline:
- Reads from `data/raw/` (or a previous processed file in `data/processed/`)
- Writes a new file to `data/processed/`

This ensures full reproducibility — you can always re-run the pipeline from scratch.

---

## Column Dictionary

| Column | Description | Notes |
|--------|-------------|-------|
| `SeriousDlqin2yrs` | **Target.** 1 if borrower was 90+ days past due within 2 years, 0 otherwise | ~6.7% positive rate (imbalanced) |
| `RevolvingUtilizationOfUnsecuredLines` | Credit card balance ÷ credit limit ratio | Some values >1.0 (over-limit) and extreme outliers |
| `age` | Borrower age in years | Contains at least one row with value 0 (data entry error) |
| `NumberOfTime30-59DaysPastDueNotWorse` | Count of 30–59 day delinquencies in last 2 years | Contains sentinel values 96 and 98 — NOT real counts |
| `DebtRatio` | (Monthly debt payments + living costs) ÷ monthly gross income | Some extreme outliers (>1.0 is possible but values in thousands are errors) |
| `MonthlyIncome` | Borrower's monthly income | **~20% missing** — significant, treat as meaningful |
| `NumberOfOpenCreditLinesAndLoans` | Count of open loans and credit lines | |
| `NumberOfTimes90DaysLate` | Count of 90+ day delinquencies | Contains sentinel values 96 and 98 — NOT real counts |
| `NumberRealEstateLoansOrLines` | Count of mortgage and real estate loans | |
| `NumberOfTime60-89DaysPastDueNotWorse` | Count of 60–89 day delinquencies in last 2 years | Contains sentinel values 96 and 98 — NOT real counts |
| `NumberOfDependents` | Number of dependents (excluding borrower) | **~2.5% missing** |

---

## Known Data Quirks

These are documented here so they are handled **deliberately** in Stage 2 (data cleaning), not accidentally ignored:

1. **Missing values in `MonthlyIncome`** (~20%) — too many to drop. Imputation required. The missingness itself may be informative (borrowers who decline to report income).

2. **Missing values in `NumberOfDependents`** (~2.5%) — likely imputable with mode (0) or median.

3. **Age = 0** — at least one row. This is a data entry error. Must be filtered or imputed.

4. **Sentinel values 96 and 98** in the three past-due columns — these are placeholder codes from the data source, NOT actual delinquency counts. A borrower was not 96 times late. These must be capped or recoded before any analysis.

5. **Extreme outliers in `DebtRatio`** — some values are in the thousands, which is economically implausible. Cap at a sensible threshold.

6. **Extreme outliers in `RevolvingUtilizationOfUnsecuredLines`** — values >1.0 mean the borrower is over their credit limit (plausible), but values of 50,000+ are clearly errors.

---

## Processed Data Files

As the pipeline runs, the following files are saved to `data/processed/`:

| File | Created By | Description |
|------|-----------|-------------|
| `df_cleaned.csv` | `02_data_cleaning_feature_engineering.ipynb` | Cleaned data with imputed values, capped outliers, engineered features |
| `df_woe.csv` | `03_woe_iv_binning.ipynb` | WOE-transformed features ready for logistic regression |
| `df_scored.csv` | `07_model_comparison_export.ipynb` | Final dataset with predictions, credit scores, and risk bands |
