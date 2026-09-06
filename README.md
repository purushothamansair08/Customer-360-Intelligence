# Customer 360 Intelligence

**Brazilian E-Commerce Customer Analytics & Retention Dashboard**

A Power BI portfolio project that transforms Brazilian e-commerce transaction, customer, payment, product, seller, and review data into an executive Customer 360 view.

## Project Overview

The objective is to understand **what drives revenue, how customers purchase, where retention is at risk, and how customer satisfaction varies across products and sellers**.

The dashboard combines sales performance with customer lifecycle analysis so that business users can move from **performance monitoring → customer understanding → retention action**.

## Business Problem

The business needs a clear view of sales and customer behavior to understand:

- Revenue and order performance
- Product and seller contribution
- One-time versus repeat purchasing
- Customer churn and retention risk
- Customer satisfaction and review sentiment
- High-value customers and repeat-purchase opportunities

## Objectives

1. **Sales Performance** — Understand revenue, orders, products and state performance.
2. **Customer Behavior** — Compare one-time and repeat purchasing patterns.
3. **Retention** — Identify churn and opportunities to encourage repeat purchases.
4. **Satisfaction** — Analyze reviews across products, categories and sellers.
5. **Growth Opportunities** — Identify high-value customers and retention opportunities.

## Dataset

**Brazilian E-Commerce Public Dataset by Olist**

The model contains approximately:

| Area | Scale |
|---|---:|
| Orders | ~100K |
| Unique customers | ~99K |
| Products sold | ~113K |
| Reviews | ~98K |

The dataset covers sales, customers, payments, products, sellers, orders and customer reviews.

## Data Source

**Dataset Source:** Kaggle

**Original Dataset:** Brazilian E-Commerce Public Dataset by Olist

The original dataset was obtained from Kaggle and transformed in Power BI for analytical use.

## Data Preparation

The raw dataset was prepared using **Power Query** before being used for dashboard development.

The preparation process included:

- Data type validation
- Data cleaning
- Column selection
- Data transformation
- Relationship preparation
- Customer-level data preparation
- Product and category preparation
- Date preparation
- Structuring tables for analytical use

### Data Quality Issues Identified

During the data preparation process, two data quality issues were identified and documented in the **Data Quality Log**.

#### DQ-001 — Missing Delivered Dates

In `Fact_Orders`, **8 rows** had no `order_delivered_customer_date` even though the order status was recorded as **Delivered**.

- **Severity:** Low
- **Impact:** Delivery lead-time analysis may be affected.
- **Action:** Documented the issue and retained the source data.
- **Status:** Resolved

#### DQ-002 — Missing Product Categories

In the product dimension, `product_category_name` was missing for approximately **3% of products (623 products)**. This affected **1,627 order-item records across 1,473 unique orders**.

- **Severity:** Medium
- **Impact:** Category-level sales and order analysis may show blank categories.
- **Action:** Classified missing categories as **"Uncategorized"** for analysis while retaining the source data.
- **Status:** Resolved

The identified data quality issues and corresponding actions were recorded in the **Data Quality Log**.

The cleaned and transformed data was then loaded into Power BI and organized into a connected analytical model.

## Data Model

The Power BI model uses a fact/dimension structure built around the order lifecycle.

### Core Tables

- `Fact_Orders`
- `Fact_OrderItems`
- `Fact_OrderPayments`
- `Fact_OrderReviews`
- `Dim_Customers`
- `Dim_UniqueCustomer`
- `Dim_Sellers`
- `Dim_ProductCategory`
- `Dim_Date`
- `Measure`

### Analytical Flow

**Customers → Orders → Order Items → Products / Categories**

Supporting dimensions and facts provide:

- Payment analysis through order payments
- Satisfaction analysis through order reviews
- Seller analysis through order items
- Time analysis through the date dimension

## Tables Created in Power BI

Two additional tables were created to support the analytical requirements of the dashboard.

### Dim_UniqueCustomer

A customer-level dimension created from the original customer data using `customer_unique_id`.

It provides a single analytical record per unique customer and supports:

- One-time vs repeat customer analysis
- Churn classification
- Customer revenue analysis
- Customer frequency analysis
- High-value customer identification

### Measure

A dedicated table created to organize and manage DAX measures used throughout the dashboard.

## Calculated Columns Created

The following calculated columns were created to support customer and satisfaction analysis:

### Purchase Type

Classifies customers based on their number of orders:

- One-Time
- Repeat
- No Orders

### Customer State

Maps the customer's state to the unique customer dimension for customer-level geographic analysis.

### Churn Status

Classifies repeat customers based on their latest purchase activity.

### Order Frequency Group

Groups customers according to their number of orders for customer-value and purchasing-frequency analysis.

### Frequency Sort

A supporting column created to maintain the correct logical order of customer frequency groups in visuals.

### Satisfaction Level

Review scores were transformed into meaningful satisfaction categories based on the **1–5 review score range**:

| Review Score | Satisfaction Level |
|---:|---|
| 1 | Very Dissatisfied |
| 2 | Dissatisfied |
| 3 | Neutral |
| 4 | Satisfied |
| 5 | Very Satisfied |

**Review Score Range:** **1 = Very Dissatisfied → 5 = Very Satisfied**

This classification makes the numerical review scores easier to interpret for business and customer satisfaction analysis.

## Dashboard Pages

### 1. Executive Mission Control

Executive-level view of revenue, orders, customers, monthly performance, payment preferences, review status and state-level performance.

### 2. Sales Analytics

Product categories, sellers, product revenue, best-selling products, top sellers and category-level review sentiment.

### 3. Customer Analytics

Repeat customers, active repeat customers, one-time customers, churned customers, churn rate, customer purchase behavior and state-level churn.

### 4. Customer Satisfaction Distribution

Review volume, average review score, positive and negative reviews, category satisfaction rates, seller review performance and satisfaction distribution.

### 5. Customer Value & Retention Strategy

Customer value versus loyalty, revenue at risk, customer value by purchase frequency, high-value customers, repeat customer revenue and retention opportunities.

## Key Business Insights

### 1. Customer Retention Is the Major Growth Opportunity

The dataset is dominated by one-time buyers, with repeat customers representing only a small share of the customer base. This creates a significant opportunity to convert existing buyers into second-time purchasers.

### 2. Revenue Exposure Is Concentrated

Revenue-at-risk analysis highlights the states where churned customers have historically generated the most revenue. These regions can be prioritized for retention campaigns.

### 3. Customer Value Increases With Purchasing Frequency

Customers with multiple orders generally represent stronger long-term value than one-time buyers. Increasing purchase frequency is therefore more actionable than focusing only on new-customer acquisition.

### 4. Satisfaction Is Broadly Positive

The dashboard shows an average review score of approximately **4.10**, with roughly **76K positive reviews** versus **14K negative reviews** out of about **98K reviews**.

### 5. Products and Sellers Are Concentrated Contributors

Top-product and top-seller analysis identifies the contributors that have an outsized influence on sales performance and can help guide commercial focus.

## Important KPI Definitions

### Total Revenue

Based on total payment value:

`SUM(Fact_OrderPayments[payment_value])`

This is different from product revenue, which is based on item prices.

### Product Revenue

Based on product item prices:

`SUM(Fact_OrderItems[price])`

Current project analysis shows product revenue of approximately **13.56M**, while payment-based revenue is approximately **16.0M**.

### Repeat Customers

Customers classified as **Repeat** plus customers classified as **Churned**. Both groups represent customers who have made more than one purchase.

### Churn

A churn classification was created to identify **repeat customers who have not made a purchase for more than 180 days** based on their latest purchase date in the dataset.

This classification is used to identify customers who are no longer actively purchasing and may require retention efforts.

### Revenue at Risk

Historical revenue generated by customers classified as churned.

It should be interpreted as **revenue exposure associated with churned customers**, not a guaranteed future loss forecast.

### High-Value Customer

For this project, a high-value customer is defined as a customer generating **more than ₹2,000 in customer revenue**.

## DAX Analytics

The dashboard uses DAX measures for customer segmentation, revenue analysis, churn analysis, satisfaction analysis and retention analysis.

### High-Value Customers

DAX
High-Value Customers =
COUNTROWS(
    FILTER(
        VALUES(Dim_UniqueCustomer[customer_unique_id]),
        [Customer Revenue] >= 2000
    )
)

### Revenue at Risk

Revenue at Risk =
    VAR ChurnedCustomers =
        CALCULATETABLE(
            VALUES(Dim_UniqueCustomer[customer_unique_id]),
            Dim_UniqueCustomer[Churn Status] = "Churned"
        )
    RETURN
        CALCULATE(
            [Customer Revenue],
            TREATAS(
                ChurnedCustomers,
                Dim_Customers[customer_unique_id]
            )
        )

### Repeat Customer Revenue

    Repeat Customer Revenue =
    CALCULATE(
        [Customer Revenue],
        Dim_UniqueCustomer[Purchase Type] = "Repeat"
    )

## Business Recommendations

1. **Convert One-Time Buyers**

   Launch targeted post-purchase campaigns designed to generate a second order.

2. **Protect High-Value Customers**

   Prioritize customers with higher historical revenue for personalized offers and loyalty initiatives.

3. **Focus on High-Risk States**

   Use revenue-at-risk by state to prioritize retention resources where the financial exposure is greatest.

4. **Improve Repeat-Purchase Frequency**

   Use purchase-frequency segments to create increasingly relevant campaigns instead of treating all customers the same.

5. **Monitor Satisfaction by Category and Seller**

   Investigate categories and sellers with elevated negative-review rates and address recurring service or product issues.

## Technology

- **Power BI** — Dashboard, data model and visualization
- **Power Query** — Data preparation and transformation
- **DAX** — Measures, customer segmentation and analytical calculations

## Project Limitations

- The dataset represents historical e-commerce activity and does not provide a live customer view.
- Churn is a rule-based classification, not a machine-learning prediction.
- Revenue-at-risk represents historical revenue from churned customers; it is not a forecast of exact future losses.
- The Olist dataset naturally contains a very high proportion of one-time buyers, so the retention opportunity should be interpreted within the characteristics of the source dataset.
- Payment and review analysis depends on the available relationships between orders, payments, items, sellers and reviews.
- Missing delivered dates and product categories identified in the Data Quality Log may affect specific delivery and category-level analyses.

## Portfolio Deliverables

    Customer-360-Intelligence/

    ├── Power BI/
    │   └── Customer 360 Intelligence.pbix
    │
    ├── Presentation/
    │   └── Customer 360 Intelligence.pptx
    │
    ├── Documentation/
    │   ├── Customer-360-Intelligence-README.md
    │   └── Customer-360-Intelligence-Report.pdf
    │
    └── Screenshots/
        ├── Executive Mission Control.png
        ├── Sales Analytics.png
        ├── Customer Analytics.png
        ├── Customer Satisfaction.png
        └── Customer Value & Retention Strategy.png

## Final Outcome

This project demonstrates the ability to take a multi-table e-commerce dataset, perform data preparation and quality assessment, build a connected analytical model, create reusable DAX measures, design an executive dashboard, and translate customer and sales data into practical retention and growth recommendations.

**Tools:**  Power BI • Power Query • DAX

**Dataset Source:**  Kaggle

**Original Dataset:**  Brazilian E-Commerce Public Dataset by Olist

**Project:**  Customer 360 Intelligence

**Year:**  2026