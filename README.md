# CreditScope: An End-to-End Credit Risk Scorecard & Default Prediction Engine

> **Portfolio project** targeting Financial Data Analyst, Risk & Analytics Analyst, and FP&A/BI Analyst roles at Big 4 firms, bulge bracket banks, and companies across Japan, Singapore, UAE, and UK/Europe.

---

## Business Problem

Given an applicant's financial and credit history, predict the probability of **serious delinquency** (90+ days past due) within the next two years, and translate that probability into:

1. A **credit score** (points-based scorecard, similar to FICO) that is interpretable and explainable to non-technical stakeholders such as credit committees.
2. A **risk segmentation** (Low / Medium / High risk bands) that a lending team could use to set approval thresholds or pricing tiers.
3. A **portfolio stress-test view** showing total expected loss exposure across risk segments.

---

## Dataset

**Give Me Some Credit** — [Kaggle competition](https://www.kaggle.com/c/GiveMeSomeCredit/data)

- ~150,000 borrower records
- Binary target: `SeriousDlqin2yrs` (1 = defaulted within 2 years)
- Approximately 6.7% default rate (class imbalanced)

See [`data/README.md`](data/README.md) for the full column dictionary and download instructions.

---

## Project Structure

```
creditscope-credit-risk-scorecard/
├── data/
│   ├── raw/                    cs-training.csv (gitignored)
│   ├── processed/              cleaned/transformed data between stages
│   └── README.md               download instructions + column dictionary
├── notebooks/                  numbered pipeline, run in order (00–07)
│   ├── 00_environment_check.ipynb
│   ├── 01_eda.ipynb
│   ├── 02_data_cleaning_feature_engineering.ipynb
│   ├── 03_woe_iv_binning.ipynb
│   ├── 04_logistic_regression_scorecard.ipynb
│   ├── 05_xgboost_model.ipynb
│   ├── 06_shap_explainability.ipynb
│   └── 07_model_comparison_export.ipynb
├── src/                        reusable Python functions (imported by notebooks)
├── models/                     saved model artifacts (.pkl, .json) (gitignored)
├── reports/figures/            exported charts and plots
├── dashboard/                  Power BI file and dashboard build notes
└── docs/notes/                 concept notes explaining the why behind each technique
```

---

## Pipeline Overview

| Stage | Notebook | What It Does |
|-------|----------|--------------|
| 0 | `00_environment_check` | Verify packages, load data, confirm shape and default rate |
| 1 | `01_eda` | Exploratory data analysis — distributions, outliers, correlations, default rate by feature |
| 2 | `02_data_cleaning_feature_engineering` | Imputation, outlier capping, sentinel value handling, new feature creation |
| 3 | `03_woe_iv_binning` | Weight of Evidence & Information Value — credit-risk-specific feature transformation |
| 4 | `04_logistic_regression_scorecard` | Industry-standard interpretable model → points-based scorecard |
| 5 | `05_xgboost_model` | Gradient boosting challenger model for higher predictive power |
| 6 | `06_shap_explainability` | Global & local SHAP explanations — the centrepiece differentiator |
| 7 | `07_model_comparison_export` | Side-by-side comparison, final scoring, export for Power BI |
| 8 | Power BI dashboard | Executive summary, score distribution, stress testing, borrower drillthrough |

---

## Evaluation Metrics

Because the target is imbalanced (~6.7% default rate), standard accuracy is meaningless. This project uses industry-standard credit risk metrics:

| Metric | What It Measures | Target |
|--------|-----------------|--------|
| **AUC-ROC** | Overall ranking ability of the model | > 0.75 |
| **KS Statistic** | Maximum separation between default and non-default distributions | > 0.30 |
| **Gini Coefficient** | AUC rescaled to 0–1 range (Gini = 2 × AUC − 1) | > 0.50 |

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| Python 3.10+ | All modelling and analysis |
| pandas, numpy | Data manipulation |
| scikit-learn | Preprocessing, model evaluation |
| statsmodels | Logistic regression with full statistical output |
| xgboost | Gradient boosting challenger model |
| optbinning | WOE/IV binning (credit-risk-specific) |
| shap | Model explainability |
| matplotlib, seaborn | Visualisation |
| openpyxl | Excel export for Power BI input |
| Power BI Desktop | Business intelligence dashboard |

---

## Quick Start

```bash
# 1. Clone the repo
git clone https://github.com/<your-username>/creditscope-credit-risk-scorecard.git
cd creditscope-credit-risk-scorecard

# 2. Create a virtual environment
python -m venv venv
venv\Scripts\activate        # Windows
# source venv/bin/activate   # macOS/Linux

# 3. Install dependencies
pip install -r requirements.txt

# 4. Download the dataset
# See data/README.md for instructions
# Place cs-training.csv in data/raw/

# 5. Run the pipeline
# Open notebooks/ in Jupyter and run 00 → 07 in order
jupyter notebook notebooks/
```

---

## Key Differentiators

- **WOE/IV binning** — a credit-risk-specific technique that most ML portfolio projects skip entirely
- **Points-based scorecard** — translates model output into a format credit committees actually use
- **SHAP explainability** — both global (portfolio-level) and local (individual borrower) explanations
- **Portfolio stress-test view** — "what happens to our loss exposure if unemployment rises?"
- **Business-first framing** — every output speaks the language of risk, not the language of ML

---

## License

This project is for educational and portfolio purposes.
