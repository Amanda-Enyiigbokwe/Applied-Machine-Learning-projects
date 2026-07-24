# Decision Tree Classification Models

## Project Overview

This project focuses on training, evaluating, and interpreting supervised machine learning Decision Tree classification models across two independent business contexts. Both parts apply the same core methodology, data cleaning, preprocessing, model training, complexity analysis, feature importance, and evaluation using multiple metrics, but differ in their business objectives, datasets, and real-world implications.

The shared learning goal across both parts is to understand how Decision Tree models make predictions, how to control model complexity to prevent overfitting, how to interpret feature importance in a business context, and how to communicate model results responsibly to stakeholders.

---

## Repository Structure

```
Decision Tree Classification Models/
├── README.md                                       
│
├── Part 1 – SalesBoost CRM Analytics/
│   ├── notebook_part 1.ipynb
│   ├── salesboost_b2b_deal_win_dataset.csv
│   ├── SalesBoost CRM Analytics Business Report.pdf
│   ├── README.md
│
└── Part 2 – ReturnWise Retail Analytics/
    ├── notebook_part 2.ipynb
    ├── returnwise_product_return_risk_dataset.csv
    ├── ReturnWise Retail Analytics Business Report.pdf
    └── README.md
```

---

## Project Parts at a Glance

| | **Part 1 – SalesBoost CRM Analytics** | **Part 2 – ReturnWise Retail Analytics** |
|---|---|---|
| **Business Goal** | Predict whether a B2B sales lead will result in a won deal | Predict whether a customer order will be returned |
| **Target Variable** | `Deal_Won` (Yes / No) | `Returned` (Yes / No) |
| **Dataset** | `salesboost_b2b_deal_win_dataset.csv` | `returnwise_product_return_risk_dataset.csv` |
| **Records (after cleaning)** | 260 (4 duplicates removed) | 450 (5 duplicates removed) |
| **Train / Test Split** | 208 / 52 | 360 / 90 |
| **Processed Features** | 54 (after OHE of 11 categorical features) | 46 (after OHE of 9 categorical features) |
| **Model** | Decision Tree (`max_depth=4`, Gini) | Decision Tree (`max_depth=4`, Gini) |
| **Testing Accuracy** | 75.0% | 66.7% |
| **F1-Score (positive class)** | 74.5% | 21.1% |
| **Priority Metric** | Recall and F1-Score | Recall and F1-Score |
| **Top Feature** | `Lead_Score` (65.5%) | `Size_Fit_Issue_Level_High` (13.9%) |
| **Overfitting Gap (unconstrained)** | ~30.8 pts (100% train / 69.2% test) | ~40 pts (100% train / 60.0% test) |

---

## Shared Methods

Both parts follow the same machine learning pipeline:

- **Data Cleaning:** Removed duplicate rows; dropped non-predictive identifiers; imputed missing numerical values with the column median and missing categorical values with the column mode.
- **Preprocessing:** Applied `OneHotEncoder` to categorical features via a `ColumnTransformer`; numerical features passed through unchanged as Decision Trees do not require feature scaling.
- **Train/Test Split:** 80/20 split with `stratify=y` and `random_state=42` to ensure reproducible, class-balanced evaluation.
- **Model Configuration:** Decision Tree Classifier with `max_depth=4`, `criterion='gini'`, and `random_state=42`; a controlled variant with `min_samples_leaf=5` was also evaluated to reduce overfitting.
- **Evaluation:** Confusion matrix, accuracy, precision, recall, F1-score, and a full classification report on the held-out test set.
- **Complexity Analysis:** Training vs. testing accuracy swept across `max_depth` values (1–10) plus an unconstrained tree to identify the overfitting threshold.
- **Feature Importance:** Gini-based importance scores extracted and ranked to surface the top business drivers of each prediction outcome.

---

## Key Takeaways

**Part 1 – SalesBoost CRM Analytics** produced a strong-performing model with a 74.5% F1-score. Feature importance was highly concentrated `Lead_Score` alone accounted for 65.5% of all splits, meaning the model relies heavily on a single derived field. This concentration is both a strength (the model is simple and interpretable) and a risk (if Lead_Score data quality is poor, the model's predictions degrade rapidly).

**Part 2 – ReturnWise Retail Analytics** revealed a more challenging prediction problem. Despite 66.7% testing accuracy, the model achieved only 14.8% recall on the return class, catching just 4 of 27 actual returns in the test set. This reflects a class imbalance (72% No / 28% Yes) that caused the tree to heavily favour the majority class. Unlike Part 1, feature importance was distributed across ten meaningful predictors, with `Size_Fit_Issue_Level_High`, `Delivery_Time_Days`, and `Product_Rating` leading. This Part 2 result illustrates that accuracy alone is a misleading metric for imbalanced business problems, and that recall must be prioritized when the cost of missing the positive class is high.

Across both parts, the unconstrained Decision Tree consistently overfitted to training data, achieving 100% training accuracy in both cases while generalizing poorly to unseen records. Constraining `max_depth=4` was the primary mechanism for balancing model complexity against generalization.

---

## Responsible AI — Shared Principles

Both projects raise common Responsible AI themes that apply across any real-world deployment of these models:

**Synthetic data limitations:** Both datasets were generated for teaching purposes. Real business data introduces entry errors, inconsistent field definitions, seasonal variation, and edge cases that synthetic samples cannot fully replicate. Results should be interpreted with this context in mind.

**Interpretability as an advantage:** Decision Trees are inherently transparent; the decision path for any individual prediction can be traced and explained in plain language. This is a meaningful advantage over black-box models when deploying in regulated or customer-facing business settings.

**Human review is non-negotiable:** Both models should serve as decision-support tools, not autonomous decision-makers. Sales managers and operations teams must apply human judgement before acting on predictions, particularly for edge cases, high-stakes outcomes, or predictions that conflict with contextual knowledge.

**Bias and fairness audits:** Features such as Region, Industry, and Customer_Segment are used as predictors in both models. Before real-world deployment, both models must be audited for disparate impact to ensure predictions are not unfairly targeting groups based on historical data biases rather than genuine behavioural signals.

**Privacy and data governance:** Both datasets contain sensitive commercial and customer information. In a production environment, all data must be handled in compliance with applicable privacy legislation, including PIPEDA in Canada and must never be shared publicly or uploaded to open repositories.

---

## How to Run or Review

Each part has its own detailed README with full run instructions. In summary:

**Option 1 – View on GitHub:**
Open either `.ipynb` notebook directly in GitHub. Notebooks render with code, outputs, and markdown visible without any setup.

**Option 2 – Run in Google Colab:**
Upload the relevant notebook and dataset CSV to Google Drive or a Colab session, open in [Google Colab](https://colab.research.google.com/), and run all cells from top to bottom (`Runtime > Run all`).

**Option 3 – Run Locally in Jupyter:**
```bash
pip install pandas scikit-learn matplotlib
```
Place the dataset CSV in the same directory as the notebook, launch Jupyter, and run all cells in order.

---
