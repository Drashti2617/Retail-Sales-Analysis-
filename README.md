# Retail Sales Analysis SQL Project

Project Overview

Project Title: Retail Sales AnalysisSkill Level: BeginnerDatabase Name: p1_retail_db

This project is designed to help beginners develop their SQL skills by working with retail sales data. Through this project, you will learn how to set up a database, clean data, conduct exploratory data analysis (EDA), and extract business insights using SQL queries. The project is ideal for those starting out in data analytics and looking to gain hands-on experience with SQL.

Project Objectives

Database Setup: Create and populate a database for retail sales.

Data Cleaning: Identify and remove records with missing values.

Exploratory Data Analysis: Analyze the dataset to uncover insights.

Business Insights: Use SQL queries to answer business-related questions.

Project Structure

1. Setting Up the Database

The first step is to create a database named p1_retail_db and define the structure for a table called retail_sales. This table will store sales transactions with fields like transaction ID, sale date and time, customer details, product category, quantity, price, and total sale amount.

SQL Commands to Create the Database and Table:

CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales (
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);

2. Data Exploration and Cleaning

To ensure data quality, perform the following tasks:

Count total records:

SELECT COUNT(*) FROM retail_sales;

Count unique customers:

SELECT COUNT(DISTINCT customer_id) FROM retail_sales;

Identify unique product categories:

SELECT DISTINCT category FROM retail_sales;

Check for missing values and remove invalid records:

SELECT * FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR
      gender IS NULL OR age IS NULL OR category IS NULL OR
      quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR
      gender IS NULL OR age IS NULL OR category IS NULL OR
      quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

3. Data Analysis & Business Insights

Here are some SQL queries to analyze sales data and answer key business questions:

Retrieve all sales from November 5, 2022:

SELECT * FROM retail_sales WHERE sale_date = '2022-11-05';

Find all clothing transactions with quantity > 4 in November 2022:

SELECT * FROM retail_sales
WHERE category = 'Clothing'
AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
AND quantity > 4;

Calculate total sales per category:

SELECT category, SUM(total_sale) AS net_sale, COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;

Find the average age of customers who bought beauty products:

SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';

Identify transactions where the total sale exceeds $1000:

SELECT * FROM retail_sales WHERE total_sale > 1000;

Get the number of transactions per gender and category:

SELECT category, gender, COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender;

Determine the best-selling month in each year:

SELECT year, month, avg_sale FROM (
    SELECT
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales
    GROUP BY 1, 2
) AS t1 WHERE rank = 1;

Find the top 5 customers by total sales:

SELECT customer_id, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;

Count unique customers per category:

SELECT category, COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;

Categorize sales by time of day (Morning, Afternoon, Evening):

WITH hourly_sales AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders FROM hourly_sales GROUP BY shift;

Findings and Insights

Customer Demographics: Customers of various age groups contribute to sales, with categories like Clothing and Beauty being popular.

High-Value Transactions: Some purchases exceed $1000, indicating premium sales.

Sales Trends: Monthly sales fluctuations help identify peak seasons.

Customer Behavior: Identifying top-spending customers and popular categories helps businesses tailor marketing strategies.

Reports & Summary

Sales Summary: Overview of total sales, customer distribution, and category performance.

Trend Analysis: Seasonal and hourly sales patterns.

Customer Insights: Key customers and category-wise purchase behavior.

Conclusion

This project provides an introduction to SQL for data analysts by covering database creation, data cleaning, exploratory data analysis, and business-driven SQL queries. The insights generated from this analysis can assist in making informed business decisions by understanding customer behavior and sales trends.

How to Use This Project

Clone the Repository: Download or clone the project files from GitHub.

Set Up the Database: Run the provided SQL scripts to create and populate the database.

Run Queries: Use the SQL queries provided to analyze the dataset.

Customize & Explore: Modify the queries to explore additional insights and answer more business questions.
