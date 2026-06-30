# Apple-Sales-Warranty-Analysis-using-PostgreSQL


# 🍎 Apple Sales & Warranty Analysis using PostgreSQL

<p align="center">
  <img src="Images/apple_cover.png" alt="Apple Sales Analysis Project" width="100%">
</p>

---

## 📌 Project Overview

The **Apple Sales & Warranty Analysis** project is an end-to-end SQL analytics project built using **PostgreSQL**. It simulates a real-world retail environment by analyzing sales transactions, product performance, store operations, and warranty claims.

The project is designed to answer practical business questions that organizations face daily, such as identifying high-performing stores, evaluating product lifecycle performance, tracking warranty claims, and measuring sales growth across different regions.

In addition to business analytics, the project demonstrates SQL optimization techniques by implementing indexes and evaluating query performance using **EXPLAIN ANALYZE**.

---

# 🎯 Project Objectives

- Analyze retail sales performance across stores and countries.
- Monitor product sales throughout their lifecycle.
- Evaluate warranty claim trends and repair outcomes.
- Measure year-over-year sales growth.
- Identify high-performing and underperforming products.
- Improve SQL query performance through indexing.
- Solve business scenarios using advanced PostgreSQL queries.

---

# 🛠️ Technologies Used

| Technology | Purpose |
|------------|----------|
| PostgreSQL | Database Management System |
| SQL | Data Analysis & Querying |
| pgAdmin 4 | Query Execution |
| CSV Files | Dataset |
| GitHub | Project Documentation |

---

# 🗄️ Database Schema

The project is built on a relational database consisting of five interconnected tables.

| Table | Description |
|--------|-------------|
| Category | Stores product categories |
| Products | Stores Apple product information |
| Stores | Contains store details |
| Sales | Records product sales transactions |
| Warranty | Stores warranty claim information |

---

## Entity Relationship Diagram

```md
![Schema Diagram](Images/schema_diagram.png)
```

---

# 📂 Repository Structure

```text
Apple-Sales-Data-Analysis
│
├── Dataset
│   ├── category.csv
│   ├── products.csv
│   ├── sales.csv
│   ├── stores.csv
│   └── warranty.csv
│
├── Schema
│   └── Schema.sql
│
├── SQL Scripts
│   └── Solutions.sql
│
├── Images
│   └── schema_diagram.png
│
└── README.md
```

---

# 🏗️ Database Design

## Category

Maintains product category information used to classify Apple products.

## Products

Contains product details including launch date, category, and selling price.

## Stores

Stores geographical information about Apple retail stores across multiple countries.

## Sales

Maintains transactional sales records including store, product, sale date, and quantity sold.

## Warranty

Contains warranty claim records linked to individual sales transactions along with repair status.

---

# 📊 Exploratory Data Analysis (EDA)

Before solving business problems, exploratory analysis was performed to understand the dataset.

### Activities Performed

- Examined data distribution across all tables.
- Identified distinct warranty repair statuses.
- Validated record counts.
- Verified foreign key relationships.
- Reviewed product launch dates.
- Checked sales transaction volumes.

---

# ⚡ Query Performance Optimization

To improve query execution time, indexes were created on high-frequency search columns.

### Indexes Created

- Product ID
- Store ID
- Sale Date

Performance was evaluated using **EXPLAIN ANALYZE** before and after indexing to compare execution time improvements.

---

# 📈 Business Problems Solved

The project includes **20 business-oriented SQL problems** ranging from basic reporting to advanced analytical scenarios.

## Easy to Medium

1. Find the number of stores in each country.
2. Calculate the total units sold by each store.
3. Identify sales during December 2023.
4. Find stores without warranty claims.
5. Calculate the percentage of rejected warranty claims.
6. Identify the highest-selling store in 2023.
7. Count unique products sold during 2023.
8. Calculate average product price by category.
9. Count warranty claims filed in October 2020.
10. Determine the best-selling day for every store.

---

## Medium to Hard

11. Identify the least-selling product in each country.
12. Calculate warranty claims filed within 180 days of purchase.
13. Determine warranty claims for products launched within the last two years.
14. Find months where USA sales exceeded 5,000 units.
15. Identify the product category with the highest warranty claims.

---

## Advanced SQL Problems

16. Calculate warranty claim probability by country.
17. Analyze year-over-year sales growth for each store.
18. Analyze the relationship between product price and warranty claims.
19. Identify the store with the highest percentage of pending warranty claims.
20. Calculate monthly running sales totals for each store.

---

## Bonus Analysis

### Product Lifecycle Analysis

Segmented product sales into lifecycle stages:

- Launch to 6 Months
- 6–12 Months
- 12–18 Months
- Beyond 18 Months

This analysis helps evaluate how product demand changes throughout its lifecycle.

---

# 💻 SQL Concepts Demonstrated

- SQL Joins
- Aggregate Functions
- CASE Statements
- GROUP BY & HAVING
- Subqueries
- Common Table Expressions (CTEs)
- Window Functions
- DENSE_RANK()
- LAG()
- Date & Time Functions
- Query Optimization
- EXPLAIN ANALYZE
- Indexing

---

# 📌 Key Business Insights

### Sales Insights

- Identified the highest-performing stores.
- Measured annual sales growth.
- Evaluated monthly sales trends.
- Compared sales performance across countries.

### Product Insights

- Analyzed product lifecycle performance.
- Identified low-performing products.
- Measured average pricing by category.

### Warranty Insights

- Evaluated warranty claim rates.
- Identified categories with the highest warranty frequency.
- Measured pending and rejected repair percentages.
- Compared warranty risk across countries.

### Performance Optimization

- Improved SQL query execution using indexes.
- Benchmarked query performance with EXPLAIN ANALYZE.

---

# 🚀 Skills Demonstrated

### Technical Skills

- PostgreSQL
- SQL
- Query Optimization
- Database Design
- Indexing
- Data Analysis

### Analytical Skills

- Sales Analytics
- Warranty Analytics
- Product Performance Analysis
- Business Intelligence
- Trend Analysis
- Performance Reporting

---

# 📚 Learning Outcomes

This project enhanced my understanding of:

- Relational database design
- PostgreSQL query writing
- Query optimization techniques
- Business-oriented data analysis
- Sales and warranty reporting
- Product lifecycle analysis
- Advanced SQL problem-solving

---

# 🔮 Future Enhancements

- Build an interactive Power BI dashboard.
- Develop executive KPI dashboards.
- Create geographical sales visualizations.
- Add forecasting for future sales trends.
- Perform predictive analysis on warranty claims.

---

# 👨‍💻 Author

**Vrushabh Borkar**

Aspiring Business Analyst | Data Analyst

**Skills:** PostgreSQL • SQL • Power BI • Excel • Business Analytics

---

## ⭐ If you found this project helpful, consider giving it a star!
