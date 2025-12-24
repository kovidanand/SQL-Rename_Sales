# Retail Sales Analysis SQL Project
## Project Overview
**Project Title:** Retail Sales Analysis    
**Level:** Beginner   
**Database:** retail_sales    

This project demonstrates SQL skills for exploring, cleaning, and analyzing retail sales data through structured business questions and advanced analytics.

## Objectives
1. **Database Setup:** Create and populate retail sales table structure

2. **Data Cleaning:** Identify and handle null/missing values

3. **Exploratory Analysis:** Understand dataset dimensions and patterns

4. **Business Intelligence:** Answer 10 specific business questions with SQL queries

## Database Setup
Create the retail_sales table to store transaction data:
```sql
 CREATE TABLE retail_sales (
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(20),
    age INT,
    category VARCHAR(20),
    quantiy INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
```
## Data Exploration & Cleaning
Basic Metrics:
```sql
-- Total sales records
SELECT COUNT(*) AS total_sales FROM retail_sales;

-- Unique customers
SELECT COUNT(DISTINCT customer_id) AS total_customers FROM retail_sales;

-- Product categories
SELECT DISTINCT category FROM retail_sales;

-- Null value check
SELECT * FROM retail_sales
WHERE transactions_id IS NULL OR sale_date IS NULL OR sale_time IS NULL 
   OR customer_id IS NULL OR gender IS NULL OR age IS NULL 
   OR category IS NULL OR quantiy IS NULL OR price_per_unit IS NULL 
   OR cogs IS NULL OR total_sale IS NULL;
```
## Business Questions & Solutions
1. **Sales on specific date (2022-11-05)**
```sql
SELECT * FROM retail_sales
WHERE sale_date = '2022-11-05';
```
2. **Clothing transactions >3 units in Nov-2022**
```sql
SELECT * FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantiy >= 3;
```
3. **Total sales by category**
```sql
SELECT category, SUM(total_sale) as net_sale, COUNT(*) as total_orders
FROM retail_sales
GROUP BY category;
```
4. **Average age of Beauty category customers**
```sql
SELECT ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty';
```
5. **High-value transactions (>1000)**
```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000;
```
6. **Transactions by gender and category**
```sql
SELECT category, gender, COUNT(*) as total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
```
7. **Best selling month per year**
```sql
SELECT year, month, avg_sale
FROM (
    SELECT
        EXTRACT(YEAR FROM sale_date) as year,
        EXTRACT(MONTH FROM sale_date) as month,
        AVG(total_sale) as avg_sale,
        RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) 
                    ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales
    GROUP BY 1, 2
) as t1
WHERE rank = 1;
```
8. **Top 5 customers by total sales**
```sql
SELECT customer_id, SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```
9. **Unique customers per category**
```sql
SELECT category, COUNT(DISTINCT customer_id)
FROM retail_sales
GROUP BY category;
```
10. **Orders by time shift**
```sql
WITH hourly_sales AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END as shift
    FROM retail_sales
)
SELECT shift, COUNT(*) as total_orders
FROM hourly_sales
GROUP BY shift;
```
## Key SQL Features Demonstrated
1. **Date Functions:** EXTRACT, TO_CHAR for temporal analysis
​

2. **Window Functions:** RANK() for ranking best months
​

3. **CTEs:** Common Table Expressions for shift analysis
​

4. **Aggregations:** SUM, COUNT, AVG with GROUP BY
​

5. **Conditional Logic:** CASE statements for business categorization
​

## Usage Instructions
1. Execute table creation script
​

2. Import retail sales CSV data
​

3. Run data quality checks
​

4. Execute business questions sequentially
​

5. Compatible with PostgreSQL (uses PostgreSQL-specific functions)
​

## Technical Notes
1. **Database:** PostgreSQL recommended
​

2. **Data Volume:** Suitable for 10K+ transaction records
​

3. **Performance:** Optimized GROUP BY and window functions
​

4. **Extensibility:** Ready for joins with customer/inventory tables
​


































