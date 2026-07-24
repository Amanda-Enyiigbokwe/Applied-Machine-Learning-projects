# Model Evaluation, Explainability, and Fairness

## Project Overview

This project evaluates a Decision Tree classification model end-to-end on a real business problem. The goal is to predict, at the point of submission, whether a specific claim is likely to be approved or denied. By scoring incoming claims in advance, claims teams can prioritize reviewer time, flag higher-risk submissions for additional scrutiny, and apply more consistent approval criteria across customers and regions.

The analysis goes beyond accuracy to assess explainability and fairness. It evaluates the model using multiple classification metrics, explains predictions using SHAP and LIME, assesses whether the model performs equitably across demographic and geographic subgroups, and concludes with a business-oriented recommendation on whether the model is ready for real-world claims use.


## Dataset Description

**File:** `insurance_claim_approval_dataset.csv`     
**Records:** 380       
**Columns:** 19

This dataset contains insurance claim records. Each row represents one submitted claim. Features include customer profile information (age, customer tenure, customer segment), policy and premium details (policy type, annual premium, coverage amount, payment history score), vehicle and driver information (vehicle age, driving experience), and claim details (previous claims, claim amount, accident severity, police report filed, claim processing days). Three group-related columns: `gender`, `age_group`, and `region` are retained for fairness analysis but excluded from model input.

## Target Variable

`claim_approved`: Binary classification label
- **1** = Claim approved
- **0** = Claim not approved

Class distribution: **209 approved (55.0%), 171 not approved (45.0%)** mildly imbalanced.

## Model Used

**Decision Tree Classifier (scikit-learn)**
Final (controlled) model: `max_depth=4`
Preprocessing: Pipeline + ColumnTransformer with `SimpleImputer` + `OneHotEncoder` fitted on training data only to prevent data leakage.

## Main Evaluation Results

| Metric | Score |
|---|---|
| Accuracy | 61.84% |
| Precision | 0.6275 |
| Recall | 0.7619 |
| F1-Score | 0.6882 |
| CV Mean Accuracy (5-fold) | 56.58% |
| CV Std Dev | 0.0336 |
| Training Accuracy | 78.62% |
| Testing Accuracy | 61.84% |

All metrics are reported for the controlled tree (`max_depth=4`), the final model used for all evaluations.

**Note:** The single-split test accuracy (61.8%) is meaningfully higher than the 5-fold cross-validation mean (56.6%), and the CV score sits only 1.6 percentage points above the 55.0% naive majority-class baseline. The CV score is the more conservative and reliable estimate of generalization performance, and indicates the model has very limited predictive power on genuinely new claims.

## Main Business Interpretation

This model predicts whether submitted insurance claims are likely to be approved, enabling claims teams to triage incoming submissions, prioritize reviewer time, and apply more consistent approval criteria across customers and regions.

The most important predictive features are: **payment history score, customer tenure years, whether a police report was filed, driving experience years, and annual premium**. These reflect that established insurer-trust signals — customers with strong payment histories, long relationships, third-party-verified incidents, and experienced drivers are the strongest predictors of approval. Notably, the claim amount itself plays a much smaller role than might be expected.

The most costly model error is a **False Positive** predicting "approved" when the claim should have been denied because each False Positive translates directly into a payout on an invalid claim, with possible fraud exposure and reserve mis-estimation. The model produced 19 False Positives against 10 False Negatives, leaning in the more financially damaging direction. The model should be tuned to minimize False Positives (higher Precision) in any production setting, with a target of ≥0.80 before deployment.

SHAP analysis confirms that `payment_history_score`, `customer_tenure_years`, and `police_report_filed` are consistently the dominant drivers across the test set. LIME provides individual-level explanations that allow claims adjusters to audit specific predictions and produce defensible justifications for appeals or regulatory inquiries.

## Fairness and Bias

The model's performance was evaluated across three group columns: `gender`, `region`, and `age_group`. Material disparities were observed:

- **18–25 age group:** 41.7% accuracy (worse than random) with a 25-point over-approval gap.
- **51+ age group:** 63.0% accuracy with a 30-point over-approval gap (the largest in the analysis).
- **West region:** 55.6% accuracy with a 22-point over-approval gap.
- **Gender:** 6-point accuracy gap between Female (58.1%) and Male (64.4%) claimants.

Although these columns were excluded from training, the model has clearly learned them indirectly through correlated features (proxy bias). The model should not be used to make autonomous decisions for any group. All model outputs should serve as decision support for claims adjusters, not as automated approval or denial.

## Limitation

The dataset contains only 380 records, which limits the reliability of the model — especially for subgroup fairness analysis, where some groups have fewer than 20 test records (e.g., the 18–25 group has only 12, the North region has only 13). The model also overfits the training data by 16.8 percentage points (78.6% training vs 61.8% testing accuracy), and the CV mean of 56.6% sits barely above the naive baseline. The model should be retrained on a larger, more representative sample (target ≥1,500 records), with stronger algorithms (Random Forest, Gradient Boosting) and formal fairness mitigation, before any production deployment.

## How to Run

**Option 1 — View on GitHub:**
Open `explainability_notebook.ipynb` directly in GitHub. Notebooks render with code, outputs, and markdown visible without any setup.

**Option 2 — Run in Google Colab:**
Upload `explainability_notebook.ipynb` and `insurance_claim_approval_dataset.csv` to a Colab session, then run all cells from top to bottom (`Runtime > Run all`).

**Option 3 — Run Locally in Jupyter:**
```bash
pip install pandas scikit-learn matplotlib
```
Place the dataset CSV in the same directory as the notebook, launch Jupyter, and run all cells in order.

---
