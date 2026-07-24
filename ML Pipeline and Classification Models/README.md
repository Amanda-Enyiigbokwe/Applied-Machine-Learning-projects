# ML Pipeline and Classification Models

## Project Overview

This project involved building an end-to-end machine learning pipeline using a real-world dataset. The goal was to practise the full workflow of a data science project: loading and inspecting data, understanding the business problem, cleaning and preprocessing the data, training a baseline classification model, and evaluating its performance using standard metrics. The project also required a written explanation of the model, dataset, results, and limitations to demonstrate understanding of the work completed.

---

## Files in This Folder

- `notebook.ipynb`: main notebook containing the full ML pipeline, model training, evaluation, and written explanation

---

## Dataset or Input Source

**Dataset:** Investment Product Subscription Dataset (`investment_product_subscription_dataset.xls`)

This dataset contains demographic, financial, and behavioural information about bank customers, along with a label indicating whether each customer subscribed to an investment product. 

- **Original size:** 377 rows, 13 columns
- **After cleaning (duplicates and missing values removed):** 370 rows, 13 columns
- **Target variable:** `Subscribed_Investment_Product` (Yes / No)

---

## Methods Used

- **Data Loading & Inspection**: loaded the `.xls` file using pandas; inspected shape, data types, and value distributions
- **Business Problem Definition**: identified this as a binary classification problem (will a customer subscribe: Yes or No?)
- **Feature & Target Selection**: selected 11 features (5 numerical, 6 categorical) and 1 binary target variable
- **Data Cleaning**: removed 7 duplicate rows; handled 7 missing values per affected column using median (numerical) and mode (categorical) imputation; corrected data types
- **Train/Test Split**: 80/20 stratified split (296 training rows, 74 testing rows)
- **Preprocessing**: applied `StandardScaler` to numerical features; applied `OneHotEncoder` to categorical features via a `ColumnTransformer` pipeline; feature space expanded from 11 to 21 columns after encoding
- **Baseline Model**: trained a Logistic Regression model (`max_iter=1000`) on the processed training data
- **Evaluation Metrics**: Accuracy, Precision, Recall, F1-Score, Confusion Matrix, and full Classification Report

---

## Key Results

- **Accuracy: 81.1%**: the model correctly predicted the subscription outcome for 60 out of 74 test customers
- **Recall: 87.2%**: the model successfully identified 34 out of 39 customers who genuinely would subscribe, missing only 5 real subscribers (False Negatives)
- **Precision: 79.1%**: when the model predicted a customer would subscribe, it was correct approximately 79% of the time; 9 customers were incorrectly flagged as subscribers (False Positives)
- **F1-Score: 82.9%**: a strong balanced score indicating the model performs well on both catching real subscribers and avoiding false alarms
- **Confusion Matrix:** 26 True Negatives, 34 True Positives, 9 False Positives, 5 False Negatives, showing the model is better at catching subscribers (high recall) than at ruling out non-subscribers (lower recall of 74% for the "No" class)

---

## Responsible AI Reflection

**Limitations:**
The dataset is small (370 records), which means the evaluation metrics can shift significantly with just a few prediction changes. This makes it difficult to generalize conclusions confidently to a real customer population.

**Possible Bias:**
The dataset may not represent the full diversity of a real bank's customer base. Features such as `Employment_Status` (which includes categories like "Student") and `Annual_Income` could introduce socioeconomic bias, causing the model to systematically underserve or misclassify certain demographic groups.

**Privacy Concerns:**
The dataset contains sensitive personal and financial information, including age, annual income, and account balance. In a real deployment, this data would need to comply with relevant privacy regulations (such as PIPEDA in Canada or GDPR in Europe), and strict access controls, data anonymization, and consent frameworks would be required before any model could be trained or deployed.

**Interpretability:**
Logistic Regression is one of the more interpretable machine learning models. Its coefficients can indicate which features push a prediction toward "Yes" or "No." However, after one-hot encoding expands the features to 21 columns, the output becomes harder to communicate to non-technical business stakeholders without additional explanation tools.

**Human Review:**
Before deploying this model in a real banking context, human review would be essential. The 5 missed subscribers (False Negatives) and 9 false alarms (False Positives) carry real business costs, missed revenue and wasted marketing spend, respectively. A model of this size and simplicity should be treated as a starting point only. Final decisions about targeting customers for financial products should involve human oversight, compliance review, and fairness auditing before any automated system is used.

---

## How to Run or Review

**Option 1 — View directly on GitHub:**
Open the `.ipynb` file in this repository. GitHub renders Jupyter Notebooks automatically, displaying all code, outputs, and written explanations without needing to run anything.

**Option 2 — Run in Google Colab:**
1. Download the `.ipynb` file from this repository
2. Go to [colab.research.google.com](https://colab.research.google.com) and upload the file
3. Upload the dataset file when prompted, or update the file path in the notebook
4. Click **Runtime → Run all** to execute all cells

**Option 3 — Run locally in Jupyter:**
1. Clone this repository or download the `.ipynb` file
2. Ensure the following libraries are installed:
```bash
pip install pandas numpy scikit-learn openpyxl xlrd matplotlib seaborn
```
3. Launch Jupyter Notebook and open the file
4. Place the dataset in the same directory as the notebook, then run all cells

---
