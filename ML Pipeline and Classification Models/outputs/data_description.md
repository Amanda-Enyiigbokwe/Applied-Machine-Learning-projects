# Dataset Description

## Dataset Name
Investment Product Subscription Dataset

## File Name
`investment_product_subscription_dataset.xls`

---

## Dataset Overview

| Property | Details |
|---|---|
| Format | Microsoft Excel (.xls) |
| Original Size | 377 rows, 13 columns |
| After Cleaning | 370 rows, 13 columns |
| Task Type | Binary Classification |
| Target Variable | `Subscribed_Investment_Product` (Yes / No) |

---

## Business Context

The dataset simulates a bank's customer records and is designed to support a binary classification problem: **predicting whether a customer will subscribe to an investment product.** This type of prediction is commonly used in banking and financial services to improve the efficiency of marketing campaigns by targeting customers most likely to respond positively.

---

## Columns and Descriptions

| Column | Type | Description |
|---|---|---|
| `Customer_ID` | Integer | Unique identifier for each customer (not used as a feature) |
| `Age` | Numerical | Customer's age in years |
| `Employment_Status` | Categorical | Employment type (e.g. Full-time, Part-time, Self-employed, Student) |
| `Annual_Income` | Numerical | Customer's annual income |
| `Account_Balance` | Numerical | Customer's current bank account balance |
| `Risk_Tolerance` | Categorical | Customer's self-reported risk appetite (Low, Medium, High) |
| `Previous_Investments` | Categorical | Whether the customer has made investments before (Yes / No) |
| `Advisor_Contacted` | Categorical | Whether a financial advisor contacted the customer (Yes / No) |
| `Mobile_Banking_User` | Categorical | Whether the customer uses mobile banking (Yes / No) |
| `Financial_Literacy_Score` | Numerical | A score (1–10) reflecting the customer's financial knowledge |
| `Marketing_Email_Opened` | Categorical | Whether the customer opened a marketing email (Yes / No) |
| `Number_of_Bank_Products` | Numerical | Number of bank products the customer currently holds |
| `Subscribed_Investment_Product` | Categorical | **Target variable** — whether the customer subscribed (Yes / No), encoded as 1 / 0 |

---

## Data Quality Notes

The following data quality issues were identified and resolved during preprocessing:

- **Missing Values:** 7 rows had missing values across the following columns: `Age`, `Employment_Status`, `Annual_Income`, `Account_Balance`, `Risk_Tolerance`, and `Marketing_Email_Opened`. These were imputed using the **median** for numerical columns and the **mode** for categorical columns.
- **Duplicate Rows:** 7 fully duplicate rows were identified and removed.
- **Data Types:** Several columns required type correction after loading — numerical columns were cast to `int` or `float`, and categorical columns were cast to the `category` dtype for efficiency.

After cleaning, the dataset contained **370 rows and 13 columns** with no remaining missing values or duplicates.

---

## Features Used in the Model

**Input Features (X) — 11 columns:**
- Numerical (5): `Age`, `Annual_Income`, `Account_Balance`, `Financial_Literacy_Score`, `Number_of_Bank_Products`
- Categorical (6): `Employment_Status`, `Risk_Tolerance`, `Previous_Investments`, `Advisor_Contacted`, `Mobile_Banking_User`, `Marketing_Email_Opened`

**Target Variable (y) — 1 column:**
- `Subscribed_Investment_Product` → encoded as `1` (Yes) and `0` (No)

---

## Privacy and Ethical Considerations

Although this is a simulated educational dataset, it mirrors the structure of real sensitive financial data. In a real-world context, a dataset of this type would be subject to:

- **PIPEDA** (Personal Information Protection and Electronic Documents Act) in Canada
- **GDPR** (General Data Protection Regulation) if used in European jurisdictions
- Strict data anonymization, access control, and consent requirements before use in any automated decision-making system
