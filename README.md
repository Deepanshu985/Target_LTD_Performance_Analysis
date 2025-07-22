# Target_LTD_Performance_Analysis
This project combines **SQL** and **Python** to perform a comprehensive analysis for **Target Ltd**, addressing key business questions around sales performance, regional revenue gaps, and customer segmentation. It includes raw SQL queries and visual insights derived through Python.

---

## 🛠️ Tools & Technologies

- **SQL (MySQL)** – for querying and aggregating sales & customer data  
- **Python** – for visualization  
  - pandas, numpy, seaborn, matplotlib  
- **Jupyter Notebook** – for step-by-step execution  
- **GitHub** – Documentation

---


## About Business
Target Ltd is a globally recognized E-Commerce brand in the United States, known for offering exceptional valuable and a unique shopping experience.

## Objective
The primary objective of this project is to conduct a comprehensive performance analysis of Target Ltd’s operations by leveraging structured data and analytical tools. This involves:
- Systematically addressing business questions outlined by the management team
- Utilizing SQL for querying and extracting relevant insights from order, customer, and product data
- Applying Python to process and visualize complex datasets covering order status, pricing, payment behavior, shipping performance, and customer feedback
- Translating raw data into actionable business intelligence to support decision-making across sales, logistics, and customer engagement

## 🧾 SQL Queries & Insights

**1. List all unique cities where customers are located.**

```markdown
## 🔍 Business Question 1 – Top-Selling Products

To identify which products are generating the highest revenue from delivered orders:

```sql
SELECT product_name, SUM(sales_amount) AS total_sales
FROM orders
WHERE order_status = 'Delivered'
GROUP BY product_name
ORDER BY total_sales DESC;


