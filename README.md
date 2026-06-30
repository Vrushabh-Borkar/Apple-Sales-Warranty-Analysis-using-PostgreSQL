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
```sql
SELECT
	country,
	COUNT(store_id)
FROM stores 
GROUP BY 1
ORDER BY 2 DESC;
```
2. Calculate the total units sold by each store.
```sql
SELECT
	str.store_name,
	SUM(sl.quantity) AS qty_sold
FROM stores AS str
JOIN sales AS sl
ON str.store_id = sl.store_id
GROUP BY 1
ORDER BY 2 DESC;
```
3. Identify sales during December 2023.
```sql
-- Approch 1 -

SELECT
	COUNT(sale_id) AS total_sale
	FROM sales
WHERE TO_CHAR(sale_date, 'MM-YYYY') = '12-2023' ;

-- Approch 2 -

SELECT 
	COUNT(sale_id) AS total_sale
FROM sales
WHERE EXTRACT(MONTH FROM sale_date) = 12
AND
	EXTRACT(YEAR FROM sale_date) = 2023 ;
```
4. Find stores without warranty claims.
```sql
SELECT COUNT(*)
FROM stores
WHERE store_id NOT IN(
					SELECT
						DISTINCT(s.store_id)
					FROM warranty AS w
					LEFT JOIN sales AS s
					ON w.sale_id = s.sale_id
					);
```
5. Calculate the percentage of rejected warranty claims.
```sql
SELECT
	ROUND(COUNT(claim_id)/((SELECT COUNT(claim_id) FROM warranty) :: NUMERIC) * 100, 2) AS rejection_pt
FROM warranty
WHERE repair_status = 'Rejected';

```
6. Identify the highest-selling store in 2023.
```sql
SELECT
	str.store_id,
	str.store_name,
	SUM(s.quantity) AS total_sales
FROM sales s
JOIN stores str
ON str.store_id = s.store_id
WHERE EXTRACT(YEAR FROM s.sale_date)= 2023
GROUP BY 1,2
ORDER BY 3 DESC
LIMIT 1;
```
7. Count unique products sold during 2023.
```sql
SELECT
	COUNT(*) AS total_unique_products_sold
FROM
(
SELECT
	DISTINCT s.product_id,
	p.product_name
FROM products AS p
JOIN sales AS s
ON p.product_id = s.product_id
WHERE EXTRACT(YEAR FROM s.sale_date) = 2023
);

```
8. Calculate average product price by category.
```sql
SELECT
	c.category_id,
	c.category_name,
	ROUND((AVG(p.price)::NUMERIC),2) AS average_price
FROM category AS c
LEFT JOIN products AS p
ON c.category_id = p.category_id 
GROUP BY 1,2
ORDER BY 3 DESC;
```
9. Count warranty claims filed in October 2020.
```sql
SELECT 
	COUNT(*) AS repair_claim
FROM warranty
WHERE EXTRACT(MONTH FROM claim_date) = 10;

```
10. Determine the best-selling day for every store.
```sql
SELECT * 
FROM
(
	SELECT
		str.store_id,
		str.store_name,
		TO_CHAR(s.sale_date, 'DAY') AS day_name,
		SUM(s.quantity) AS total_qty_sold,
		DENSE_RANK() OVER(PARTITION BY str.store_id ORDER BY SUM(s.quantity) DESC) AS ranks
	FROM sales AS s
	JOIN stores AS str
	ON str.store_id = s.store_id
	GROUP BY 1,2,3
) AS t1
WHERE ranks = 1
```
---

## Medium to Hard

11. Identify the least-selling product in each country.
```sql
SELECT *
FROM
(
SELECT
	str.country,
	P.product_name,
	SUM(s.quantity) AS total_qty_sold,
	DENSE_RANK() OVER(PARTITION BY 	str.country ORDER BY SUM(s.quantity) ASC) AS ranks
FROM sales AS s
JOIN stores AS str
ON str.store_id = s.store_id
JOIN products AS p
ON p.product_id = s.product_id
GROUP BY 1, 2
) AS t1
WHERE ranks = 1;

```
12. Calculate warranty claims filed within 180 days of purchase.
```sql
SELECT COUNT(*) AS claims_within_180_days
FROM warranty AS wc
JOIN sales s 
    ON wc.sale_id = s.sale_id
WHERE EXTRACT(YEAR FROM s.sale_date) = 2024
  AND wc.claim_date <= s.sale_date + INTERVAL '180 DAY';

```
13. Determine warranty claims for products launched within the last two years.
```sql
SELECT
    p.product_id,
    p.product_name,
    COUNT(w.claim_id) AS total_warranty_claims
FROM products p
JOIN sales s
    ON p.product_id = s.product_id
JOIN warranty w
    ON s.sale_id = w.sale_id
WHERE p.launch_date >= CURRENT_DATE - INTERVAL '2 years'
GROUP BY p.product_id, p.product_name
ORDER BY total_warranty_claims DESC ;
```
14. Find months where USA sales exceeded 5,000 units.
```sql
SELECT

	TO_CHAR(s.sale_date, 'MM-YYYY') AS months,
	SUM(s.quantity) AS total_qty_sold
FROM sales AS s
JOIN stores AS str
ON str.store_id = s.store_id
WHERE 
	str.country = 'United States'
	AND
	s.sale_date >= CURRENT_DATE - INTERVAL '3 YEAR'
GROUP BY 1
HAVING SUM(s.quantity) > 5000 ;
```
15. Identify the product category with the highest warranty claims.
```sql
SELECT
	c.category_name,
	COUNT(w.claim_id) AS total_claims
FROM warranty AS w
LEFT JOIN sales AS s
ON w.sale_id = s.sale_id
JOIN products AS p
ON p.product_id = s.product_id
JOIN category AS c
ON c.category_id = p.category_id
WHERE w.claim_date >= CURRENT_DATE - INTERVAL '3 YEAR'
GROUP BY 1
ORDER BY 2 DESC ;
```
---

## Advanced SQL Problems

16. Calculate warranty claim probability by country.
```sql
SELECT
	country,
	total_qty,
	total_claim,
	COALESCE((total_claim::NUMERIC)/(total_qty::NUMERIC) * 100, 0) AS risk_ptg
FROM
	(SELECT
		str.country,
		SUM(s.quantity) AS total_qty,
		COUNT(w.claim_id) AS total_claim
	FROM sales AS s
	JOIN stores AS str
	ON s.store_id = str.store_id
	LEFT JOIN warranty AS w
	ON s.sale_id = w.sale_id
	GROUP BY 1) AS t1
ORDER BY 4 DESC ;
```
17. Analyze year-over-year sales growth for each store.
```sql
WITH yearly_sales
AS
	(
	SELECT
		str.store_id,
		str.store_name,
		EXTRACT(YEAR FROM sale_date) AS year_name,
		SUM(s.quantity * p.price) AS total_sale
	FROM sales AS s
	JOIN products AS p
	ON p.product_id = s.product_id
	JOIN stores AS str
	ON str.store_id = s.store_id
	GROUP BY 1, 2, 3
	ORDER BY 1, 3
	),
growth_ratio
AS
	(SELECT
		store_name,
		year_name,
		total_sale AS current_year_sale,
		LAG(total_sale, 1) OVER(PARTITION BY store_name ORDER BY year_name) AS last_year_sale
	FROM yearly_sales
	)
	
SELECT
	store_name,
	last_year_sale,
	current_year_sale,
	ROUND((current_year_sale - last_year_sale)::NUMERIC/ last_year_sale ::NUMERIC * 100, 2) AS growth_ratio
FROM growth_ratio
WHERE last_year_sale IS NOT NULL ;
```
18. Analyze the relationship between product price and warranty claims.
```sql
SELECT
	CASE
		WHEN p.price < 500 THEN 'Primary Segment'
		WHEN p.price BETWEEN 500 AND 1000 THEN 'Mid Segment'
		ELSE 'Premium Segment'
	END AS price_segment,
	COUNT(w.claim_id) AS total_claim
FROM warranty AS w
LEFT JOIN sales AS s
ON w.sale_id = s.sale_id
JOIN products AS p
ON p.product_id = s.product_id
WHERE w.claim_date >= CURRENT_DATE - INTERVAL '5 YEAR'
GROUP BY 1 ;

-- Primary Segment - 5089
-- Mid Segment - 8121
-- Premium Segment - 16790
```
19. Identify the store with the highest percentage of pending warranty claims.
```sql

WITH pendings
AS
(
SELECT
	str.store_id,
	str.store_name,
	COUNT(w.claim_id) AS total_pendings
FROM warranty AS w
LEFT JOIN sales AS s
ON w.sale_id = s.sale_id
JOIN stores AS str
ON s.store_id = str.store_id
WHERE w.repair_status = 'Pending'
GROUP BY 1, 2
),

total_claim_ids
AS
(
SELECT
	str.store_id,
	str.store_name,
	COUNT(w.claim_id) AS total_claims
FROM warranty AS w
LEFT JOIN sales AS s
ON w.sale_id = s.sale_id
JOIN stores AS str
ON s.store_id = str.store_id
GROUP BY 1, 2
)

SELECT
	tc.store_id,
	tc.store_name,
	pd.total_pendings,
	tc.total_claims,
	ROUND(pd.total_pendings::NUMERIC/ tc.total_claims::NUMERIC * 100, 2) AS ptg_pendings
FROM pendings AS pd
JOIN total_claim_ids AS tc
ON pd.store_id = tc.store_id ;
```
20. Calculate monthly running sales totals for each store.
```sql
WITH monthly_sales
AS
(
SELECT
	s.store_id,
	EXTRACT(MONTH FROM s.sale_date) AS months,
	EXTRACT(YEAR FROM s.sale_date) AS years,
	SUM(p.price * s.quantity) AS total_revenue
	
FROM sales AS s
JOIN stores AS str
ON str.store_id = s.store_id
JOIN products AS p
ON p.product_id = s.product_id
GROUP BY 1,2,3
ORDER BY 1,2,3
) 
SELECT *,
	SUM(total_revenue) OVER(PARTITION BY store_id ORDER BY years, months) AS running_total
FROM monthly_sales ;
```
---

## Bonus Analysis

### Product Lifecycle Analysis

Segmented product sales into lifecycle stages:

- Launch to 6 Months
- 6–12 Months
- 12–18 Months
- Beyond 18 Months
```sql
SELECT
	p.product_id,
	p.product_name,
	
	CASE
		WHEN s.sale_date 
			BETWEEN p.launch_date AND p.launch_date + INTERVAL '6 MONTH' 
				THEN '0-6 Months'
		WHEN s.sale_date 
			BETWEEN p.launch_date + INTERVAL '6 MONTH' AND p.launch_date + INTERVAL '12 MONTH' 
				THEN '6-12 Months'
		WHEN s.sale_date 
			BETWEEN p.launch_date + INTERVAL '12 MONTH' AND p.launch_date + INTERVAL '18 MONTH' 
				THEN '12-18 Months' 
		ELSE '18+ Months'
	END AS PLC,
	
	SUM(s.quantity) AS total_qty_sold
	
FROM sales AS s
JOIN products AS p
ON p.product_id = s.product_id
GROUP BY 1, 2, 3
ORDER BY 1, 2, 4 DESC ;
```


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
