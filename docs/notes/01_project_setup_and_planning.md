# Concept Note 01 — Project Setup & Planning

> **Stage:** Pre-pipeline  
> **Date:** 2026-08-07  
> **Purpose:** Document the why behind the project design before writing any code.

---

## Why This Project Exists

Most data analyst portfolios showcase generic machine learning projects — Titanic survival prediction, house price regression, sentiment analysis. These projects demonstrate coding ability, but they don't demonstrate **domain expertise** or **business-oriented thinking**.

CreditScope is designed differently. It targets a specific industry (financial services / credit risk) and solves a specific business problem (predicting borrower default and translating that prediction into actionable tools for a lending team). Every output is framed in the language that credit risk professionals actually use: scorecards, risk bands, portfolio loss exposure, KS statistics.

The goal is not to build the highest-performing model. The goal is to demonstrate the ability to **translate data into business decisions** — which is the core skill that Big 4 firms and banks hire for.

---

## Why This Dataset

The **Give Me Some Credit** dataset from Kaggle was chosen because:

1. **It's a real credit risk dataset** with realistic features (utilisation ratios, delinquency counts, debt ratios) — not a toy dataset.
2. **It has class imbalance** (~6.7% default rate), which forces thoughtful metric selection and evaluation strategy — a key skill in risk analytics.
3. **It has messy data** (missing values, sentinel codes, outliers) that requires deliberate cleaning decisions — not a clean CSV that loads perfectly.
4. **It's well-known in credit risk circles**, so interviewers who've worked in the space will recognise it and can assess the quality of the analysis.

---

## Why This Pipeline Structure

The 8-stage pipeline is deliberately ordered to mirror how a credit risk model is built in industry:

1. **EDA first** — understand the data before touching it.
2. **Cleaning & feature engineering** — deliberate decisions about imputation, outlier handling, and new features, all documented.
3. **WOE/IV binning** — this is the technique that separates a credit risk project from a generic ML project. Weight of Evidence transforms features into a form that's optimal for logistic regression, and Information Value quantifies each feature's predictive power. Most portfolio projects skip this entirely.
4. **Logistic regression scorecard** — the industry standard for interpretable credit scoring. Banks still use these because regulators require model explainability. The points-based scorecard is the deliverable that a credit committee would actually review.
5. **XGBoost as a challenger model** — demonstrates awareness of modern methods and provides a performance benchmark, but isn't the primary deliverable.
6. **SHAP explainability** — bridges the gap between the black-box XGBoost model and the interpretability requirement. This is the centrepiece differentiator.
7. **Model comparison & export** — professional-grade output that feeds into the Power BI dashboard.
8. **Power BI dashboard** — the final business deliverable. Recruiters at Big 4 firms want to see BI tool proficiency, not just Python notebooks.

---

## Why These Metrics (Not Accuracy)

With a ~6.7% default rate, a model that predicts "no default" for every borrower would achieve ~93.3% accuracy. That model is completely useless.

In credit risk, the correct metrics are:

- **AUC-ROC**: Measures the model's ability to rank defaulters above non-defaulters across all probability thresholds. An AUC of 0.5 is random; 1.0 is perfect. Industry scorecards typically achieve 0.70–0.85.

- **KS Statistic** (Kolmogorov-Smirnov): The maximum vertical distance between the cumulative distribution of defaults and non-defaults. This is the most commonly reported metric in scorecard validation. A KS of 0.3+ is considered good.

- **Gini Coefficient**: Simply `Gini = 2 × AUC − 1`. It rescales AUC to a 0–100% range, making it more intuitive for business stakeholders. A Gini of 0% is useless; 100% is perfect.

These three metrics are standard in credit risk reporting at banks and financial institutions worldwide. Using them (instead of accuracy, precision, or F1) signals domain fluency to the interviewer.

---

## Key Design Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Raw data immutability | Never modify files in `data/raw/` | Ensures full reproducibility from scratch |
| Notebook numbering | `00_` through `07_` | Enforces execution order; anyone can follow the pipeline |
| Business language first | Comments and markdown in risk language | Audience is finance recruiters, not ML engineers |
| Reusable functions in `src/` | Imported, not copy-pasted | Professional code organisation; avoids notebook bloat |
| Both LR and XGBoost | Champion-challenger framework | Shows interpretability-performance tradeoff awareness |
| SHAP, not just feature importance | Global + local explanations | Regulatory-grade explainability, not just a bar chart |
| Power BI as final output | Not a Streamlit app or HTML report | Matches the BI tools Big 4 firms and banks actually use |

---

## What's Next

With the project scaffold in place, the immediate next steps are:

1. ✅ Verify the environment loads all packages (`00_environment_check.ipynb`)
2. ➡️ Begin exploratory data analysis (`01_eda.ipynb`)
