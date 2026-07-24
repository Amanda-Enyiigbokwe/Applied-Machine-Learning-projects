# Classification Models and Evaluation Metrics

## Project Overview

This project focuses on training, evaluating, and comparing two supervised machine learning classification models. The goal is to predict whether a bank customer will subscribe to an investment product based on their demographic, financial, and behavioural information. The notebook covers the full machine learning pipeline: data cleaning, preprocessing, model training, evaluation using multiple metrics, and business interpretation of the results.

---

## Files in This Folder

- `classificationmodels_notebook.ipynb`: Main notebook for the task. It includes data preparation, model training (Logistic Regression and SVM), evaluation metrics, confusion matrices, and final interpretation.
- `outputs/`: Figures and screenshots including confusion matrix plots and the model comparison bar chart.
- `data_description.md`: Dataset source and notes.

---

## Dataset or Input Source

**Dataset:** `investment_product_subscription_dataset.xls`

This is a bank marketing dataset where each row represents a customer. The target variable is `Subscribed_Investment_Product` (Yes/No), indicating whether a customer subscribed to an investment product. Features include customer demographics, financial indicators, and behavioural data such as:

- Age, Employment Status, Annual Income, Account Balance
- Risk Tolerance, Previous Investments, Number of Bank Products
- Financial Literacy Score, Advisor Contacted, Mobile Banking User, Marketing Email Opened

---

## Methods Used

- **Data Cleaning:** Removed duplicate rows; filled missing numerical values with the median and missing categorical values with the mode.
- **Feature Engineering:** Separated features (X) and target variable (y); encoded the target as binary (No = 0, Yes = 1).
- **Train/Test Split:** 80/20 split with `stratify=y` to preserve class proportions (`random_state=42`).
- **Preprocessing:** Applied `StandardScaler` to numerical features and `OneHotEncoder` to categorical features using a `ColumnTransformer`.
- **Models Trained:**
  - Logistic Regression (`max_iter=1000`, `random_state=42`)
  - Support Vector Machine / SVC (`random_state=42`)
- **Evaluation Metrics:** Confusion matrix, accuracy, precision, recall, F1-score, and classification report for both models.
- **Model Comparison:** Side-by-side metric table and bar chart; best model selected by highest F1-score.

---

## Key Results

- **Logistic Regression achieved the highest F1-score** and was identified as the better-performing model for this dataset based on the automated comparison in the notebook.
- Both models were evaluated on the same held-out test set (20% of data) using accuracy, precision, recall, and F1-score to ensure a fair comparison.
- **Recall** is the most critical metric for this business problem, as missing a customer who would have subscribed (false negative) is more costly than contacting one who wouldn't (false positive).
- The Logistic Regression model also provides predicted probabilities for each customer, offering interpretable confidence scores that support more nuanced decision-making.
- The conclusion code dynamically identifies the best model using `idxmax()` on the F1-score column, making the comparison reproducible across different dataset runs.

---

## Responsible AI Reflection

**Limitations:**
The models are trained on historical data and may not capture recent shifts in customer behaviour or market conditions. No resampling techniques (e.g., SMOTE) were applied to address potential class imbalance, which could bias models toward predicting "No Subscription" more frequently.

**Possible Bias:**
Features such as age, employment status, and income are used as predictors. If these attributes are correlated with protected demographic groups, the model could unintentionally discriminate in who gets targeted for financial products, raising fairness concerns.

**Privacy Concerns:**
The dataset contains sensitive financial and personal customer information (income, account balance, risk profile). This data must be handled in compliance with applicable privacy regulations and should never be shared publicly or uploaded to open repositories.

**Interpretability:**
Logistic Regression is relatively interpretable feature coefficients can indicate which factors most influence subscription likelihood. SVM, however, is less transparent, which limits explainability in a regulated financial context.

**Human Review:**
Human judgment should be used before deploying this model in a real business setting. The model should serve as a decision-support tool, not an autonomous decision-maker. A human reviewer should validate edge cases, monitor for model drift over time, and ensure compliance with financial services regulations that require explainable, fair, and auditable customer-facing decisions.

---

## How to Run or Review

**Option 1 – View on GitHub:**
Open `week03_notebook.ipynb` directly in GitHub. GitHub renders Jupyter notebooks with code, outputs, and markdown cells visible without any setup required.

**Option 2 – Run in Google Colab:**
1. Upload `week03_notebook.ipynb` and `investment_product_subscription_dataset.xls` to your Google Drive or Colab session.
2. Open the notebook in [Google Colab](https://colab.research.google.com/).
3. Run all cells from top to bottom (`Runtime > Run all`).

**Option 3 – Run Locally in Jupyter:**
1. Install required libraries if not already installed:
   ```bash
   pip install pandas scikit-learn matplotlib
   ```
2. Place the dataset file in the same directory as the notebook.
3. Launch Jupyter and open `week03_notebook.ipynb`.
4. Run all cells in order.

---
