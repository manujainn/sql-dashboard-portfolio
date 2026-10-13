# 📊 Enterprise Business Intelligence & SQL Analytics Project

## 🔍 Project Overview
This project simulates an end-to-end Business Intelligence initiative focused on extracting actionable business insights from relational databases. By leveraging advanced SQL queries and dynamic dashboard visualizations, this project evaluates key operational metrics, customer segmentation patterns, and financial trends to drive data-backed decision-making.

---

## 🏗️ Technical Architecture & Tech Stack
- **Database Management / Querying:** SQL (MySQL / PostgreSQL / SQL Server)
- **Data Visualization & BI:** Power BI / Tableau / Advanced Excel
- **Key SQL Techniques Applied:** 
  - Common Table Expressions (CTEs)
  - Window Functions (`ROW_NUMBER()`, `RANK()`, `SUM() OVER()`)
  - Advanced Joins (Inner, Left, Right, Self-Joins)
  - Subqueries and Aggregations

---

## 📂 Repository Directory Structure
```text
├── data/                  # Sample dataset schemas and CSV extracts
├── queries/               # Modularized and commented SQL script files
├── dashboards/            # Visual exports, mockups, and dashboard screenshots
├── docs/                  # Entity Relationship Diagrams (ERD) & database schema
└── README.md              # Project documentation
-- Advanced CTE and Window Function for Tiering High-Value Customers
WITH CustomerLifetimeValue AS (
    SELECT 
        c.customer_id,
        c.customer_segment,
        SUM(t.total_amount) AS total_revenue_generated,
        COUNT(t.transaction_id) AS total_orders,
        MAX(t.transaction_date) AS last_purchase_date
    FROM customers c
    JOIN transactions t ON c.customer_id = t.customer_id
    GROUP BY c.customer_id, c.customer_segment
),
RankedCustomers AS (
    SELECT 
        customer_id,
        customer_segment,
        total_revenue_generated,
        total_orders,
        NTILE(4) OVER (ORDER BY total_revenue_generated DESC) AS revenue_quartile
    FROM CustomerLifetimeValue
)
SELECT 
    customer_id,
    customer_segment,
    total_revenue_generated,
    total_orders,
    CASE 
        WHEN revenue_quartile = 1 THEN 'Tier 1 - VIP'
        WHEN revenue_quartile = 2 THEN 'Tier 2 - Core'
        WHEN revenue_quartile = 3 THEN 'Tier 3 - Regular'
        ELSE 'Tier 4 - Low Engagement'
    END AS customer_tier
FROM RankedCustomers
ORDER BY total_revenue_generated DESC;


