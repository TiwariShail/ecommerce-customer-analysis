# ecommerce-customer-analysis
End-to-end Python data analysis — E-Commerce Customer Behaviour, RFM Segmentation &amp; Repeat Purchase Prediction
# 🛒 E-Commerce Customer Analysis & Repeat Purchase Prediction

![Python](https://img.shields.io/badge/Tool-Python-blue?style=flat&logo=python)
![Pandas](https://img.shields.io/badge/Library-Pandas-green?style=flat)
![Scikit-Learn](https://img.shields.io/badge/ML-Scikit--Learn-orange?style=flat)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=flat)
![Stages](https://img.shields.io/badge/Stages-Cleaning%20%7C%20EDA%20%7C%20RFM%20%7C%20ML-purple?style=flat)

## 📌 Project Overview

An end-to-end data analysis project on an **E-Commerce Orders & Customer dataset** containing 1154 rows of intentionally dirty data. The project covers five complete phases — Data Cleaning, EDA, Data Analysis, RFM Customer Segmentation, and Machine Learning — entirely using **Python (pandas, matplotlib, seaborn, scikit-learn)**.

The goal was to simulate a real-world e-commerce analyst workflow: clean raw messy data, explore patterns, segment customers using RFM methodology, and build a predictive model to identify which customers will make repeat purchases.

---

## 🗂️ Dataset Description

| Property | Detail |
|---|---|
| Source | Synthetic dataset with real-world dirty data patterns |
| Raw Rows | 1154 |
| Clean Rows | 1138 |
| Unique Customers | 400 |
| Unique Orders | 1127 |
| Time Period | June 2022 — May 2024 |
| Domain | E-Commerce — Orders & Customer Behaviour |

**Key Columns:** Order_ID, Customer_ID, Age, Gender, City, Category, Product, Quantity, Unit_Price, Discount, Total_Amount, Payment_Method, Order_Date, Delivery_Date, Delivery_Days, Order_Status, Rating, Return_Reason, Customer_Since

---

## ❌ Data Quality Issues Found & Fixed

| Issue | Rows Affected | Fix Applied |
|---|---|---|
| Duplicate records | 20 | Removed exact duplicates |
| Invalid Age values | 8 | Replaced with median |
| Negative Unit_Price | 6 | Recovered using formula |
| Invalid Discount (>1) | 7 | Recovered using formula |
| Order_Status typos | 49 | Corrected (Deliverd → Delivered etc.) |
| Bad Delivery dates | 10 | Dropped unrecoverable rows |
| Mixed date formats | ~57 | Unified using pd.to_datetime(format=mixed) |
| NULL Payment_Method | 40 | Recovered using Customer_ID cross-reference |
| NULL Gender | ~20 | Filled using mode |
| Junk column | 1 column | Dropped (100% NULL) |
| Total_Amount mismatch | 13 | Recalculated from Price × Qty × (1-Discount) |

---

## 🧹 Data Cleaning Highlights

**Payment Method Recovery — Advanced Technique 🌟**
Instead of filling NULL payment methods with mode, used Customer_ID to find the same customer's payment method from other orders — a real-world data recovery technique.

**Total_Amount Consistency Check**
Cross-validated every row: `Total_Amount == Unit_Price × Quantity × (1 - Discount)` and fixed 13 mismatched rows.

**Formula-Based Recovery**
Recovered invalid Unit_Price and Discount values by reverse engineering from Total_Amount — same approach used by professional data teams.

---

## 🔍 EDA — Key Findings

| Area | Finding |
|---|---|
| Top Category | Electronics — highest revenue |
| Top Region | West (Pune, Jaipur, Ahmedabad, Mumbai) — 52% revenue |
| Top Age Group | 36-50 — highest orders and spending |
| Best Year | 2023 outperformed 2022 in all metrics |
| Avg Delivery | 8 days across all cities |
| Return Rate | ~15% — Defective Product most common reason |
| Payment | Cash on Delivery most used method |
| Top City | Pune — highest order volume |

---

## 📊 RFM Customer Segmentation

RFM (Recency, Frequency, Monetary) analysis was performed on 400 unique customers to segment them by loyalty and value:

| Segment | Customers | % | Strategy |
|---|---|---|---|
| 🔴 At Risk | 132 | 33% | Win-back campaigns urgently |
| 🟡 Needs Attention | 104 | 26% | Re-engagement offers |
| 🟢 Loyal Customers | 82 | 20% | Reward and retain |
| 🔵 Potential Loyalists | 61 | 15% | Nurture with targeted offers |
| 🏆 Champions | 21 | 5% | VIP treatment — highest value |

**Key Insight:** 33% of customers are At Risk — urgent retention action needed.

---

## 🤖 Machine Learning — Repeat Purchase Prediction

**Business Question:** Can we predict which customers will make a repeat purchase based on their first order behaviour?

**Target Variable:** `Repeat_Buyer` (1 = ordered more than once, 0 = one-time buyer)
**Repeat Rate in dataset:** 84.8%

### Model Journey — 3 Versions

**Version 1 — Data Leakage Detected ❌**
```
Initial model: 100% Accuracy, AUC = 1.000
Red flag → Total_Orders used as feature but
           Repeat_Buyer = (Total_Orders > 1)
           This is direct data leakage!
```

**Version 2 — Leakage Fixed, Insufficient Features ⚠️**
```
LR: 47.50% Accuracy, AUC = 0.554
DT: 56.25% Accuracy, AUC = 0.581
First order data alone not enough to predict repeat behaviour
```

**Version 3 — Feature Engineering Added ✅ Final**
```
New features: Days_As_Customer, Category_Repeat_Rate,
              First_Order_Delivered, High_Discount

LR: 72.50% Accuracy, AUC = 0.788 ← BEST MODEL
DT: 46.25% Accuracy, AUC = 0.612
```

### Final Model Results

| Metric | Logistic Regression | Decision Tree |
|---|---|---|
| Accuracy | **72.50%** | 46.25% |
| Precision | **94.23%** | 87.88% |
| Recall | **72.06%** | 42.65% |
| F1 Score | **81.67%** | 57.43% |
| ROC AUC | **0.788** | 0.612 |

### Top Features for Prediction

| Rank | Feature | Importance | Insight |
|---|---|---|---|
| 1 | Days_As_Customer | 0.373 | Longer customer = more likely to repeat |
| 2 | Total_Amount | 0.213 | Higher first order = more invested |
| 3 | Age | 0.087 | Demographics matter |
| 4 | Discount | 0.082 | Price sensitivity affects loyalty |
| 5 | Delivery_Days | 0.068 | Faster delivery = higher retention |

---

## 💡 Business Recommendations

1. **At Risk segment (33%)** — Launch immediate win-back campaigns with personalised offers
2. **Champions (5%)** — Implement VIP program — they drive disproportionate revenue
3. **Improve delivery speed** — Delivery_Days is a top predictor of repeat purchase
4. **Electronics strategy** — Top revenue category needs premium after-sales service
5. **West region focus** — 52% revenue from West — expand operations in Pune and Ahmedabad
6. **Early engagement** — Days_As_Customer is top ML feature — engage customers in first 30 days

---

## 📁 Repository Structure

```
ecommerce-customer-analysis/
│
├── README.md
│
├── notebooks/
│   └── Ecommerce_Analysis.ipynb    ← Complete analysis
│
├── data/
│   ├── Ecommerce_Dirty_Data.csv    ← Raw dataset
│   └── Ecommerce_Clean_Data.csv    ← After cleaning
│
└── charts/
    ├── rating_distribution.png
    ├── category_revenue.png
    ├── rfm_segments.png
    ├── ml_confusion_matrix.png
    └── ml_roc_curve.png
```

---

## 🛠️ Tools & Libraries

| Tool | Purpose |
|---|---|
| **Python 3.x** | Core language |
| **pandas** | Data manipulation and cleaning |
| **numpy** | Numerical operations |
| **matplotlib** | Data visualisation |
| **seaborn** | Statistical charts |
| **scikit-learn** | ML models, evaluation metrics |
| **Jupyter Notebook** | Development environment |

---

## 💡 Skills Demonstrated

| Skill | Where Used |
|---|---|
| Data Cleaning | 15+ step systematic process |
| Formula-based Recovery | Unit_Price, Quantity, Total_Amount |
| Cross-table Recovery | Payment_Method via Customer_ID |
| RFM Segmentation | Customer loyalty analysis |
| Feature Engineering | Days_As_Customer, Category_Repeat_Rate |
| Data Leakage Detection | Identified and fixed 100% accuracy trap |
| ML Model Iteration | 3-version journey with documented reasoning |
| Business Thinking | Recommendations tied to data findings |

---

## 📬 Connect With Me

- **LinkedIn:** [Add your LinkedIn URL]
- **Email:** [Add your email]

---

*This project was built independently as part of a hands-on Python data science learning journey.*
*Domain expertise from e-commerce background helped generate meaningful business insights.*
