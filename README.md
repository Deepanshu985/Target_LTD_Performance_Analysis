# E-Commerce Business Performance Analysis (SQL + Python)

---

## Project Overview

This project presents an end-to-end **data analytics solution** for a simulated e-commerce company, designed to analyze business performance across sales, customers, products, and sellers.

The analysis focuses on transforming raw transactional data into **actionable business insights** that support decision-making in revenue optimization, customer retention, and operational efficiency.

---

## Business Context

This project simulates a real-world **e-commerce business scenario**, similar to large-scale platforms operating in the US.

The company deals with:
- High volumes of customer orders  
- Multiple product categories  
- Diverse seller ecosystem  
- Various payment methods  

Despite having large amounts of data, the business lacks clarity on:
- Sales performance trends  
- Customer behavior and retention  
- Revenue contribution by categories and sellers  
- Regional demand distribution  

---

## Objective

The primary objective of this project is to:

- Analyze end-to-end business performance using structured data  
- Answer key stakeholder-driven business questions  
- Identify trends in sales, customer behavior, and product performance  
- Evaluate seller contribution and revenue distribution  
- Provide actionable insights to improve business outcomes  

---

## Tools & Technologies

- **SQL (MySQL)** - Data extraction and analysis  
- **Python** - Data processing and visualization  
- **Libraries** - pandas, numpy, matplotlib, seaborn  
- **Jupyter Notebook** - Analysis workflow  
- **GitHub** - Documentation  

---

## Data Analysis Workflow

### 1. Data Extraction (SQL)
- Joined multiple tables (orders, customers, products, payments)  
- Performed aggregations and grouping  
- Used window functions such as **RANK, LAG, and moving averages**  

---

### 2. Business Question-Based Analysis

The analysis was structured around key stakeholder questions:

---

### Customer & Geographic Analysis
- Identified unique customer cities  
- Analyzed customer distribution across states  

**Purpose:** Understand regional demand and expansion opportunities  

---

### Sales Performance Analysis
- Calculated total orders for specific years  
- Analyzed monthly order trends  
- Evaluated category-level revenue  

**Purpose:** Track business growth and seasonality  

---

### Payment Behavior Analysis
- Calculated percentage of installment-based payments  

**Insight:**  
~99.99% of transactions were made using installments, indicating strong dependence on flexible payment options  

---

### Product & Pricing Analysis
- Calculated revenue contribution by product category  
- Analyzed correlation between product price and demand  

**Insight:**  
Weak negative correlation (-0.106) suggests pricing alone does not strongly influence demand  

---

### Seller Performance Analysis
- Ranked sellers based on total revenue using window functions  

**Purpose:** Identify high-performing and underperforming sellers  

---

### Customer Behavior Analysis
- Calculated average number of products per order  
- Identified high-value customers  

---

### Advanced Analytics

- Moving average of customer purchases  
- Cumulative monthly sales  
- Year-over-year growth rate  
- Customer retention rate  

**Key Insight:**  
Customer retention within 6 months was nearly **0%**, indicating a major gap in customer engagement  

---

## Key Insights

-  Very low customer retention - poor repeat purchase behavior  
-  High reliance on installment payments - critical for conversion  
-  Weak price-demand relationship - other factors influence purchasing decisions  
-  Revenue concentrated among few sellers - dependency risk  
-  Uneven category performance - optimization opportunity  

---

## Business Recommendations

- Improve customer retention through loyalty programs and personalization  
- Focus on high-performing sellers and reduce dependency risks  
- Optimize underperforming product categories  
- Maintain and enhance installment-based payment options  
- Expand in high-performing regions  

---

## Visualization

Python libraries like **Matplotlib** and **Seaborn** were used to create:
- Bar charts  
- Trend analysis plots  
- Correlation visualizations  

These visualizations help stakeholders interpret insights quickly and effectively.

---

## Key Learnings

- Applied SQL for real-world business problem-solving  
- Used window functions for advanced analytics  
- Combined SQL and Python for end-to-end data analysis  
- Translated data into actionable business insights  

---

## Conclusion

This project demonstrates how data analytics can be used to solve real-world e-commerce challenges by:

- Structuring analysis around business problems  
- Extracting insights using SQL  
- Visualizing data using Python  
- Delivering actionable recommendations  

---
