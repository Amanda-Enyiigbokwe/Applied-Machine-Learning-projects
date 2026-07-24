# Part 2: Decision Tree Model — ReturnWise Retail Analytics

## Project Overview

This project focuses on training, evaluating, and interpreting a supervised machine learning Decision Tree classification model. The goal is to predict whether a customer order placed with a mid-sized e-commerce retailer will be returned, based on customer profile, product information, purchase behaviour, order and delivery details, and customer history data. The notebook covers the full machine learning pipeline: data cleaning, preprocessing, model training, complexity analysis, feature importance, evaluation using multiple metrics, and business interpretation of results.

---

## Files in This Folder

- `notebook_part 2.ipynb`: Main notebook for the task. It includes data preparation, Decision Tree model training, confusion matrix, evaluation metrics, overfitting analysis, feature importance, and business recommendations.
- `ReturnWise Retail Analytics Business Report.pdf`: A structured business report documenting the full analytical process and findings. It covers the business context and objectives for ReturnWise Retail Analytics, dataset overview and preprocessing steps, Decision Tree model configuration and visualization, confusion matrix interpretation, evaluation metrics, overfitting analysis, feature importance rankings, business actions and recommendations, dataset and model limitations, and Responsible AI considerations.

---

## Dataset or Input Source

**Dataset:** `returnwise_product_return_risk_dataset.csv`

This is a retail order dataset where each row represents an individual customer order. The target variable is `Returned` (Yes/No), indicating whether the order was returned. Features span five business categories:

- **Customer Profile:** Customer_Age, Customer_Segment, Region
- **Product Information:** Product_Category, Product_Price, Product_Rating, Size_Fit_Issue_Level
- **Purchase Behaviour:** Discount_Percent, Promotion_Type, Purchase_Channel, Payment_Method, Product_Page_Views
- **Order & Delivery:** Shipping_Method, Delivery_Time_Days, Items_In_Order, Return_Window_Days
- **Customer History:** Previous_Orders, Prior_Return_Count, Customer_Support_Contact

---

## Methods Used

- **Data Cleaning:** Removed 5 duplicate rows (455 → 450 records); dropped non-predictive identifiers (`Order_ID`, `Purchase_Date`); filled missing numerical values with the column median and missing categorical values with the column mode across several affected columns.
- **Feature Engineering:** Separated features (X) and target variable (y); encoded the target as binary (No = 0, Yes = 1).
- **Train/Test Split:** 80/20 split with `stratify=y` to preserve class proportions (`random_state=42`), yielding 360 training records and 90 testing records.
- **Preprocessing:** Applied `OneHotEncoder` to 9 categorical features via a `ColumnTransformer`, expanding the feature space from 19 to 46 processed features. Numerical features (10 columns) passed through unchanged — Decision Trees do not require feature scaling.
- **Model Trained:**
  - Decision Tree Classifier (`max_depth=4`, `random_state=42`, `criterion='gini'`)
  - Controlled variant: `max_depth=4`, `min_samples_leaf=5` to reduce overfitting
- **Evaluation Metrics:** Confusion matrix, accuracy, precision, recall, F1-score, and a classification report on the held-out test set.
- **Complexity Analysis:** Training vs. testing accuracy plotted across `max_depth` values (1–10) and an unconstrained tree to identify the overfitting threshold.
- **Feature Importance:** Gini-based feature importance extracted and ranked to identify the top business drivers of product return outcomes.

---

## Key Results

- **The Decision Tree (max_depth=4) achieved 66.7% testing accuracy**, correctly classifying 60 of 90 unseen orders. Precision for the positive class was 36.4%, recall was 14.8%, and the **F1-score was 21.1%** — the priority metric chosen to balance missed returns (false negatives) and unnecessary interventions (false positives).
- The model was evaluated on the same held-out test set (20% of data) using accuracy, precision, recall, and F1-score to ensure a fair and reproducible evaluation.
- **Recall is the most critical metric** for this business problem. Missing an order that will be returned (false negative) is more damaging than mistakenly flagging one that won't (false positive), as missed returns incur full reverse-logistics, refund processing, and warehouse labour costs without any early warning. The model currently catches only 4 of 27 actual returns in the test set, leaving the business largely reactive.
- **Size_Fit_Issue_Level_High dominated feature importance at 13.9%**, followed by Delivery_Time_Days (12.0%) and Product_Rating (10.4%). Other notable drivers included Region_Atlantic Canada (8.8%), Discount_Percent (7.9%), and Purchase_Channel_Website (7.9%).
- The controlled tree (`max_depth=4`, `min_samples_leaf=5`) reduced the train/test accuracy gap from 11.6 to 10.6 points, representing a marginally better-generalizing configuration.
- The unconstrained tree (`max_depth=None`) exhibited severe overfitting: 100% training accuracy versus only 60.0% testing accuracy, a 40-point gap confirming the value of depth constraints.

---

## Responsible AI Reflection

**Limitations:**
The dataset is synthetically generated for teaching purposes and contains only 450 records, far below what a production-grade model would require. Each misclassified record in the 90-record test set shifts accuracy by approximately 1.1 percentage points, making results sensitive to the random split. The dataset also excludes time-based patterns; seasonal return spikes, post-holiday behaviour, and promotional event effects were not captured because `Purchase_Date` was excluded from modelling. Real retail data contains entry errors, inconsistent field definitions, and more complex return patterns than a synthetic sample can replicate.

**Possible Bias:**
The model's recall on the return class is only 14.8%, meaning it misses 23 of 27 actual returns in the test set. The class imbalance (72% No / 28% Yes) causes the tree to favour the majority class. Features such as Region, Customer_Segment, and Product_Category are used as predictors; if certain regions or segments historically had higher return rates due to fulfilment failures rather than genuine customer behaviour, the model may have learned to unfairly target those groups. A full fairness and disparate impact audit is required before any real-world deployment.

**Privacy Concerns:**
The dataset contains sensitive customer behavioural and transactional information, including purchase channel, payment method, prior return history, and customer support interactions. In a real setting, this data must be collected and stored in compliance with applicable privacy legislation, including PIPEDA in Canada, before being used for predictive modelling or targeted operational interventions. Data should never be shared publicly or uploaded to open repositories.

**Interpretability:**
The Decision Tree is inherently interpretable — the decision path for any individual order prediction can be traced through the tree and explained in plain language to operations managers and customers. For example: "If Size_Fit_Issue_Level_High = 1 AND Prior_Return_Count > 2, predict Returned = Yes." This transparency is a meaningful advantage over black-box models in a consumer-facing context where stakeholders need to understand and trust the model's recommendations.

**Human Review:**
Human judgment must be applied before acting on model predictions. The model cannot see context outside the order data: a known product quality issue raised by a supplier, a regional logistics disruption, or the customer's specific circumstances that fall outside recorded features. Operations managers should treat the model's output as a risk-ranking tool to prioritize review of the highest-scored orders — not as an automated intervention trigger. The model should be retrained at least quarterly on fresh order data to account for model drift as product ranges, seasonal demand, supplier quality, and customer expectations evolve over time.

---

## How to Run or Review

**Option 1 – View on GitHub:**
Open `notebook_part 2.ipynb` directly in GitHub. GitHub renders Jupyter notebooks with code, outputs, and markdown cells visible without any setup required.

**Option 2 – Run in Google Colab:**
1. Upload `notebook_part 2.ipynb` and `returnwise_product_return_risk_dataset.csv` to your Google Drive or Colab session.
2. Open the notebook in [Google Colab](https://colab.research.google.com/).
3. Run all cells from top to bottom (`Runtime > Run all`).

**Option 3 – Run Locally in Jupyter:**
1. Install required libraries if not already installed:
   ```bash
   pip install pandas scikit-learn matplotlib
   ```
2. Place the dataset file (`returnwise_product_return_risk_dataset.csv`) in the same directory as the notebook.
3. Launch Jupyter and open `notebook_part 2.ipynb`.
4. Run all cells in order.

---
