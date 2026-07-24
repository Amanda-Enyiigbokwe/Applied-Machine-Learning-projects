# Applied Machine Learning Projects

---

## Overview

This repository is a portfolio of applied machine learning projects covering end-to-end pipelines, classification, clustering, and model explainability, applied to real-world business problems in sales, retail, insurance, and marketing. Each project includes a Jupyter notebook, a written business report, and a project-level README that details the business problem, methodology, results, and instructions for running the notebook.

The work reflects hands-on experience with the full analytics lifecycle: framing a business problem, preparing and engineering data, building and evaluating models, and communicating results and their limitations (including fairness and explainability considerations) to a non-technical audience.

---

## Repository Structure

```
├── projects/
│   ├── investment-product-subscription-classification/      # Classification Model and Evaluation Metrics
│   │   ├── classificationmodels_notebook.ipynb
│   │   ├── investment_product_subscription_dataset.xls
│   │   └── README.md
│   ├── decision tree classification models/
│   ├── salesboost-b2b-deal-win-prediction/                  # SalesBoost CRM Analytics: B2B Lead Qualification and Deal-Win Prediction
│   │   ├── notebook_Part 1.ipynb
│   │   ├── SalesBoost CRM Analytics Business Report.pdf
│   │   ├── salesboost_b2b_deal_win_dataset.csv
│   │   └── README.md
│   ├── returnwise-product-return-risk/                      # ReturnWise Retail Analytics: Product Return Prediction
│   │   ├── notebook_Part 2.ipynb
│   │   ├── ReturnWise Retail Analytics Business Report.pdf
│   │   ├── returnwise_product_return_risk_dataset.csv
│   │   └── README.md
│   ├── advantage-customer-segmentation/                     # AdVantage Growth Studio: K-Means Customer Segmentation
│   │   ├── kmeans_notebook.ipynb
│   │   ├── AdVantage Growth Studio Business Report.pdf
│   │   ├── marketing_campaign_decision_tree_dataset.csv
│   │   └── README.md
│   ├── investment-product-subscription-pipeline/            # ML Pipeline and Classification Models
│   │   ├── MLPipeline_notebook.ipynb
│   │   ├── investment_product_subscription_dataset.xls
│   │   └── README.md
│   ├── investment-product-subscription-classification/      # Classification Model and Evaluation Metrics
│   │   ├── classificationmodels_notebook.ipynb
│   │   ├── investment_product_subscription_dataset.xls
│   │   └── README.md
│   ├── insurance-claim-approval-explainability/             # Model Evaluation, Explainability, and Fairness
│   │   ├── nlp_notebook.ipynb
│   │   ├── Insurance Claim Approval Report.pdf
│   │   ├── insurance_claim_approval_dataset.csv
│   │   └── README.md
└── README.md
```

---

## Projects at a Glance

| Project | Business Problem | Method | Dataset |
|---|---|---|---|
| ML Pipeline Fundamentals | Introduction to building a reproducible ML pipeline | — | — |
| Investment Product Subscription — Pipeline | Predicting customer subscription to an investment product | Logistic Regression | Investment Product Subscription Dataset |
| Investment Product Subscription — Model Comparison | Comparing classifier performance for subscription prediction | Logistic Regression vs SVM | Investment Product Subscription Dataset |
| SalesBoost CRM Analytics | B2B lead qualification and deal-win prediction | Decision Tree | SalesBoost B2B Deal Win Dataset |
| ReturnWise Retail Analytics | Predicting product return risk in retail transactions | Decision Tree | ReturnWise Product Return Risk Dataset |
| AdVantage Growth Studio | Customer segmentation for targeted marketing | K-Means Clustering (k=3) | Marketing Campaign Dataset |
| Insurance Claim Approval | Model evaluation, explainability, and fairness reflection | Decision Tree + SHAP + LIME | Insurance Claim Approval Dataset |

See each project's folder README for full details on the business problem, methodology, results, and how to run the notebook.

---

## Tools and Libraries

Python, pandas, numpy, scikit-learn (Pipeline, ColumnTransformer, DecisionTreeClassifier, cross_val_score, classification metrics), SHAP, LIME, matplotlib, seaborn, Jupyter Notebook, TensorFlow/Keras, and other libraries as needed.

---

## Responsible AI & Documentation Practice

All work in this repository uses publicly available or synthetic datasets; no confidential, private, sensitive, copyrighted, or restricted data is included. Where AI tools were used to support development, their use is disclosed in the relevant project README, and all final work has been reviewed and verified.

