# Part 1: Decision Tree Model — SalesBoost CRM Analytics

## Project Overview

This project focuses on training, evaluating, and interpreting a supervised machine learning Decision Tree classification model. The goal is to predict whether a B2B sales lead at SalesBoost CRM Analytics will result in a won deal, based on lead profile, engagement behaviour, sales process activity, and commercial pipeline data. The notebook covers the full machine learning pipeline: data cleaning, preprocessing, model training, complexity analysis, feature importance, evaluation using multiple metrics, and business interpretation of results.

---

## Files in This Folder

- `notebook_part 1.ipynb`: Main notebook for the task. It includes data preparation, Decision Tree model training, confusion matrix, evaluation metrics, overfitting analysis, feature importance, and business recommendations.
- `SalesBoost CRM Analytics Business Report.pdf`: A structured business report documenting the full analytical process and findings. It covers the business context and objectives for SalesBoost CRM Analytics, dataset overview and preprocessing steps, Decision Tree model configuration and visualization, confusion matrix interpretation, evaluation metrics, overfitting analysis, feature importance rankings, business actions and recommendations, dataset and model limitations, and Responsible AI considerations.
---

## Dataset or Input Source

**Dataset:** `salesboost_b2b_deal_win_dataset.csv`

This is a B2B sales pipeline dataset where each row represents a lead in the SalesBoost CRM. The target variable is `Deal_Won` (Yes/No), indicating whether a lead became a won deal. Features span four business categories:

- **Lead & Company Profile:** Lead_Source, Industry, Region, Company_Size, Employee_Count, Annual_Revenue_Estimate, Use_Case
- **Engagement Behavior:** Website_Visits_30D, Email_Open_Rate, Demo_Requested
- **Sales Process Activity:** Sales_Calls_Count, Proposal_Sent, Budget_Confirmed, Decision_Maker_Involved, Competitor_Considered
- **Commercial & Pipeline:** Discount_Offered_Percent, Days_In_Pipeline, Estimated_Deal_Value, Lead_Score, Sales_Rep

---

## Methods Used

- **Data Cleaning:** Removed 4 duplicate rows (264 → 260 records); dropped non-predictive identifiers (`Lead_ID`, `Lead_Date`); filled missing numerical values with the column median and missing categorical values with the column mode across 7 affected columns.
- **Feature Engineering:** Converted currency columns (`Annual_Revenue_Estimate`, `Estimated_Deal_Value`) and percentage column (`Email_Open_Rate`) from string format to numeric float by stripping `$` and `%` characters. Separated features (X) and target variable (y); encoded the target as binary (No = 0, Yes = 1).
- **Train/Test Split:** 80/20 split with `stratify=y` to preserve class proportions (`random_state=42`), yielding 208 training records and 52 testing records.
- **Preprocessing:** Applied `OneHotEncoder` to 11 categorical features via a `ColumnTransformer`, expanding the feature space from 20 to 54 processed features. Numerical features (9 columns) passed through unchanged — Decision Trees do not require feature scaling.
- **Model Trained:**
  - Decision Tree Classifier (`max_depth=4`, `random_state=42`, `criterion='gini'`)
  - Controlled variant: `max_depth=4`, `min_samples_leaf=5` to reduce overfitting
- **Evaluation Metrics:** Confusion matrix, accuracy, precision, recall, F1-score, and a classification report on the held-out test set.
- **Complexity Analysis:** Training vs. testing accuracy plotted across `max_depth` values (1–10) and an unconstrained tree to identify the overfitting threshold.
- **Feature Importance:** Gini-based feature importance extracted and ranked to identify the top business drivers of deal outcomes.

---

## Key Results

- **The Decision Tree (max_depth=4) achieved 75.0% testing accuracy**, correctly classifying 39 of 52 unseen leads. Precision for the positive class was 79.2%, recall was 70.4%, and the **F1-score was 74.5%** — the priority metric chosen to balance missed revenue (false negatives) and wasted sales effort (false positives).
- The model was evaluated on the same held-out test set (20% of data) using accuracy, precision, recall, and F1-score to ensure a fair and reproducible evaluation.
- **Recall is the most critical metric** for this business problem. Missing a lead that would have become a won deal (false negative) is more damaging than mistakenly prioritizing one that would not (false positive), as slow follow-up risks losing high-potential leads to competitors.
- **Lead_Score dominated feature importance at 65.5%**, followed by Estimated_Deal_Value (13.0%) and Days_In_Pipeline (11.0%). The remaining 47 of 54 features contributed zero importance.
- The controlled tree (`max_depth=4`, `min_samples_leaf=5`) reduced leaf nodes from 14 to 12 and narrowed the train/test accuracy gap from 14.4 to 11.5 points, representing the best-balanced configuration across all depths tested.
- The unconstrained tree (`max_depth=None`) exhibited severe overfitting: 100% training accuracy versus only 69.2% testing accuracy, a 30.8-point gap confirming the value of depth constraints.

---

## Responsible AI Reflection

**Limitations:**
The dataset is synthetically generated for teaching purposes and contains only 260 records, far below what a production-grade model would require. Each misclassified record in the 52-record test set shifts accuracy by approximately 1.9 percentage points, making results sensitive to the random split. Real CRM data contains entry errors, inconsistent field definitions, and more complex behavioural patterns not captured here.

**Possible Bias:**
Lead_Score accounts for 65.5% of the model's decisions. If this derived score is computed from incomplete, inconsistently entered, or rep-subjective CRM data, the model will learn and amplify those upstream biases. Features such as Industry, Region, and Company_Size are also used as predictors; if certain groups historically received less sales attention in the training data, the model may learn to deprioritize them further — raising fairness concerns that should be audited before any real-world deployment.

**Privacy Concerns:**
The dataset contains commercially sensitive pipeline information, including estimated deal values, revenue estimates, lead scores, and sales representative data. In a real setting, this data must be handled in compliance with applicable privacy and data governance regulations and should never be shared publicly or uploaded to open repositories.

**Interpretability:**
The Decision Tree is inherently interpretable; the decision path for any individual prediction can be traced through the tree and explained in plain language to sales managers and clients. This is a meaningful advantage over black-box models in a business setting where stakeholders need to understand and trust the model's recommendations.

**Human Review:**
Human judgment must be applied before acting on model predictions. The model cannot see context outside the CRM: a warm follow-up from a prospect's CFO, a personal relationship with a decision-maker, a competitor recently losing a major client, or the strategic importance of a deal, regardless of its predicted probability. Sales managers should treat the model's output as one strong input alongside their own knowledge, flag cases where predictions feel wrong, and periodically audit recommendations to ensure the model remains fair, accurate, and aligned with how the business has evolved.

---

## How to Run or Review

**Option 1 – View on GitHub:**
Open `notebook_part 1.ipynb` directly in GitHub. GitHub renders Jupyter notebooks with code, outputs, and markdown cells visible without any setup required.

**Option 2 – Run in Google Colab:**
1. Upload `week04_notebook_part 1.ipynb` and `salesboost_b2b_deal_win_dataset.csv` to your Google Drive or Colab session.
2. Open the notebook in [Google Colab](https://colab.research.google.com/).
3. Run all cells from top to bottom (`Runtime > Run all`).

**Option 3 – Run Locally in Jupyter:**
1. Install required libraries if not already installed:
   ```bash
   pip install pandas scikit-learn matplotlib
   ```
2. Place the dataset file (`salesboost_b2b_deal_win_dataset.csv`) in the same directory as the notebook.
3. Launch Jupyter and open `notebook_part 1.ipynb`.
4. Run all cells in order.

---
