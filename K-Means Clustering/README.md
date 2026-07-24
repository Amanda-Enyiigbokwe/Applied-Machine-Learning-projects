# K-Means Clustering: Unsupervised Customer Segmentation

## Project Overview

This project applies unsupervised machine learning to a real-world marketing analytics context. Using K-Means clustering, the notebook segments customers of **AdVantage Growth Studio** into distinct behavioural groups based on purchasing history, loyalty, satisfaction, income profile, and digital engagement behaviour.

The goal is to move beyond broad, undifferentiated marketing campaigns by identifying natural customer groups that can be targeted with distinct strategies. Rather than predicting a labelled outcome (as in supervised learning), K-Means discovers hidden structure in the data without a target variable.

---

## Repository Structure

```
K-Means Clustering/
├── README.md
├── kmeans_notebook.ipynb
├── marketing_campaign_decision_tree_dataset.csv
└── AdVantage Growth Studio Business Report.pdf
```

---

## Project at a Glance

| | **AdVantage Growth Studio** |
|---|---|
| **Business Goal** | Segment customers to support targeted marketing campaign strategies |
| **Dataset** | `marketing_campaign_decision_tree_dataset.csv` |
| **Records (after cleaning)** | 600 (5 duplicates removed) |
| **Original Records** | 605 |
| **Features Used for Clustering** | 9 numerical features |
| **Method** | K-Means Clustering (unsupervised learning) |
| **Optimal k** | 3 (selected via Elbow Method) |
| **Algorithm** | KMeans (sklearn, `random_state=42`, `n_init=10`) |
| **Scaling** | StandardScaler applied to all features |

---

## Dataset Features

The dataset contains 605 records and 21 columns across six feature groups:

| Feature Group | Columns |
|---|---|
| Customer Profile | Customer_ID, Campaign_Date, Age, Annual_Income, Customer_Segment |
| Campaign Details | Campaign_Channel, Campaign_Type, Ad_Exposure_Count, Discount_Offered_Percent |
| Engagement Behaviours | Email_Opened, Ad_Clicked, Website_Visits_Last_30_Days, Social_Media_Engagement_Score |
| Purchase History | Previous_Purchases, Average_Order_Value, Days_Since_Last_Purchase |
| Loyalty and Satisfaction | Loyalty_Score, Customer_Satisfaction |
| Campaign Outcomes | Prior_Campaign_Response, Converted |

**9 numerical features selected for clustering:** Age, Annual_Income, Website_Visits_Last_30_Days, Previous_Purchases, Average_Order_Value, Days_Since_Last_Purchase, Loyalty_Score, Customer_Satisfaction, Social_Media_Engagement_Score

---

## Methodology

### Data Cleaning and Preprocessing
- Stripped extra whitespace from all column names
- Removed 5 duplicate rows (605 → 600)
- Removed `$` signs and commas from `Annual_Income` and `Average_Order_Value`; converted both from object to float
- Filled missing values in numerical columns with column medians (Annual_Income: 10 missing, Average_Order_Value: 6, Customer_Satisfaction: 8)
- Filled missing values in categorical columns with the column mode (Customer_Segment: 6 missing)
- Applied `StandardScaler` to all 9 numerical features before clustering

### Elbow Method — Selecting k

K-Means was trained for k=1 through k=10. The inertia (within-cluster sum of squared distances) was recorded at each step to identify the optimal number of clusters.

| k | Inertia | k | Inertia |
|---|---------|---|---------|
| 1 | 5,445.00 | 6 | 3,429.42 |
| 2 | 4,419.77 | 7 | 3,287.53 |
| 3 | 4,076.04 | 8 | 3,184.61 |
| 4 | 3,808.99 | 9 | 3,108.53 |
| 5 | 3,580.75 | 10 | 3,007.64 |

The largest drops occur between k=1 and k=2 (~1,026 points) and between k=2 and k=3 (~344 points). After k=3, reductions become progressively smaller, marking the elbow. **k=3 was selected** as the best balance between cluster compactness and model simplicity.

---

## Cluster Results

### Cluster Summary

| Feature | Cluster 0 (191) | Cluster 1 (227) | Cluster 2 (187) |
|---|---|---|---|
| Age | 35.78 | 40.08 | 36.83 |
| Annual Income ($) | $65,690.54 | $50,515.53 | $72,626.53 |
| Website Visits (Last 30 Days) | 6.40 | 6.04 | 6.14 |
| Previous Purchases | 6.74 | 1.80 | 1.83 |
| Average Order Value ($) | $84.13 | $68.87 | $107.44 |
| Days Since Last Purchase | 22.29 | 49.46 | 72.52 |
| Loyalty Score | 75.50 | 41.84 | 35.79 |
| Customer Satisfaction | 7.56 | 6.27 | 5.66 |
| Social Media Engagement Score | 42.20 | 51.01 | 41.27 |
| **Cluster Size** | **191 customers** | **227 customers** | **187 customers** |

### Cluster 0 — Loyal Active Customers (191 customers)
The most engaged and frequently purchasing segment. These customers hold the highest loyalty score (75.50), the highest number of previous purchases (6.74), and the highest customer satisfaction (7.56). With an average of only 22.29 days since their last purchase, they are consistently active. Despite not being the largest segment, they generate disproportionate recurring revenue and should be the primary focus of retention efforts.

### Cluster 1 — Low-Loyalty Social Browsers (227 customers)
The largest segment at 37.5% of the customer base. These customers record the highest social media engagement score (51.01) but average only 1.80 previous purchases and the lowest average order value ($68.87). Their annual income is the lowest of the three segments ($50,515.53). Marketing reach is not translating into purchasing behaviour — conversion, not awareness, is the core challenge.

### Cluster 2 — High-Income Dormant Customers (187 customers)
These customers have the highest average annual income ($72,626.53) and the highest average order value ($107.44), yet they have not purchased for an average of 72.52 days. Their loyalty score is the lowest (35.79) and their customer satisfaction is the lowest (5.66). Despite their high spending power, they are currently inactive and represent the strongest reactivation opportunity.

---

## Recommended Marketing Strategies

| Cluster | Insight | Strategy |
|---|---|---|
| Cluster 0: Loyal Active Customers | High loyalty, frequent purchases, strong satisfaction | Loyalty rewards programme; early access to new products; personalized purchase recommendations; member-only offers |
| Cluster 1: Low-Loyalty Social Browsers | High social media engagement but low conversion and few purchases | Retargeting campaigns; first-purchase discounts; social proof messaging; urgency-based calls-to-action; simplified checkout |
| Cluster 2: High-Income Dormant Customers | High income and order value but long inactivity and low satisfaction | Personalized win-back emails; satisfaction surveys; premium incentives; product recommendations based on prior purchases; VIP-style outreach |
| All clusters | Use cluster membership as a data-driven signal | Assign labels in CRM; route segments into separate workflows; monitor KPIs quarterly; refresh cluster assignments with updated data |

---

## Key Takeaways

**Engagement does not equal conversion.** Cluster 1 is the most digitally visible segment, highest social media engagement score (51.01) yet it averages the fewest purchases (1.80) and the lowest average order value ($68.87). Awareness and engagement alone are not sufficient to drive purchasing behaviour.

**The income-dormancy paradox.** Cluster 2 customers hold the highest average income ($72,626.53) and highest average order value ($107.44) but have been inactive for the longest period (72.52 days). Their low satisfaction score (5.66) suggests disengagement may be linked to prior customer experience rather than a lack of purchasing power.

**Loyal customers are a high-value minority.** Cluster 0 represents 191 customers, only 31.8% of the base but generates the most consistent recurring revenue through frequent purchases and strong loyalty. Retention of this segment is the highest-priority business objective.

**Days Since Last Purchase is the strongest differentiator.** With averages of 22.29, 49.46, and 72.52 days across the three clusters respectively, this single variable clearly separates active customers from lapsing and dormant ones.

---

## Responsible AI Considerations

**Static snapshot limitation.** The dataset captures behaviour at one point in time. A Cluster 1 browser could become a Cluster 0 loyal customer following a successful campaign. Cluster labels should be refreshed periodically using updated data.

**Excluded categorical variables.** K-Means requires numerical inputs, so valuable categorical features such as Region, Campaign_Channel, Campaign_Type, Email_Opened, and Prior_Campaign_Response were excluded. These may contain meaningful patterns not captured by the model.

**Proxy discrimination.** Cluster 1 has the lowest average income. Consistently deprioritizing this segment could unintentionally disadvantage lower-income customers. Income correlates with demographic factors, so cluster-based decisions should be audited for unintended discriminatory effects before deployment.

**Self-fulfilling prophecies.** If Cluster 2 customers receive no marketing contact because they are classified as dormant, they will never have an opportunity to re-engage. Cluster labels should inform targeted support, not justify exclusion from outreach.

**Human oversight is non-negotiable.** The clustering model surfaces patterns; marketing managers must apply judgment before acting on any cluster assignment, particularly for borderline cases or high-stakes decisions.

**Privacy and data governance.** All customer data must be handled in compliance with applicable privacy legislation, including PIPEDA in Canada, and must not be shared beyond authorized analytics and marketing teams.

---

## How to Run

**Option 1 — View on GitHub:**
Open `kmeans_notebook.ipynb` directly in GitHub. Notebooks render with code, outputs, and markdown visible without any setup.

**Option 2 — Run in Google Colab:**
Upload `kmeans_notebook.ipynb` and `marketing_campaign_decision_tree_dataset.csv` to a Colab session, then run all cells from top to bottom (`Runtime > Run all`).

**Option 3 — Run Locally in Jupyter:**
```bash
pip install pandas scikit-learn matplotlib
```
Place the dataset CSV in the same directory as the notebook, launch Jupyter, and run all cells in order.

---
