📊 Zepto Data Analysis using PostgreSQL

A complete end-to-end data exploration, cleaning, and analysis project using PostgreSQL on a dataset downloaded from Kaggle (zepto_v2.csv).

📌 Project Overview

This project focuses on analyzing product data from Zepto, an Indian instant-delivery platform.
Using PostgreSQL, I performed:

Database setup

Data ingestion

Data exploration

Data cleaning

Analytical SQL queries

Insight generation

The objective is to understand product pricing, discounts, availability trends, category-wise performance, and more.

📁 Dataset

Source: Kaggle

File: zepto_v2.csv

Contents: SKU-level product details including category, MRP, discounts, stock availability, weight, and more.

🛠️ Tech Stack

PostgreSQL 18

pgAdmin 4 / psql CLI

SQL (DDL + DML + Analytical Queries)

🏗️ Database Schema
CREATE TABLE zepto (
    sku_id SERIAL PRIMARY KEY,
    category VARCHAR(120),
    name VARCHAR(150) NOT NULL,
    mrp NUMERIC(8,2),
    discountPercent NUMERIC(5,2),
    availableQuantity INTEGER,
    discountedSellingPrice NUMERIC(8,2),
    weightInGms INTEGER,
    outOfStock BOOLEAN,
    quantity INTEGER
);

🔍 Data Exploration

Performed the following:

✔️ Row Count
SELECT COUNT(*) FROM zepto;

✔️ Sample Data
SELECT * FROM zepto LIMIT 10;

✔️ Null Value Check

Checked all major columns for missing values.

✔️ Unique Categories
SELECT DISTINCT category FROM zepto;

✔️ Stock Status Summary
SELECT outOfStock, COUNT(*) FROM zepto GROUP BY outOfStock;

✔️ Duplicate Product Names
SELECT name, COUNT(*) 
FROM zepto
GROUP BY name
HAVING COUNT(*) > 1;

🧹 Data Cleaning
✔️ Identify products with invalid price = 0
SELECT * FROM zepto WHERE mrp = 0 OR discountedSellingPrice = 0;

✔️ Remove invalid records
DELETE FROM zepto WHERE mrp = 0;

✔️ Convert paise → rupees
UPDATE zepto
SET mrp = mrp / 100.0,
    discountedSellingPrice = discountedSellingPrice / 100.0;

📈 Data Analysis & Insights
Q1. Top 10 Best-Value Products (Highest Discount%)
SELECT DISTINCT name, mrp, discountPercent
FROM zepto
ORDER BY discountPercent DESC
LIMIT 10;

Q2. High-MRP Products That Are Out of Stock

Useful for inventory planning.

Q3. Estimated Revenue per Category
SELECT category,
SUM(discountedSellingPrice * availableQuantity) AS total_revenue
FROM zepto
GROUP BY category;

Q4. High-MRP, Low-Discount Products

Identifies premium items without heavy discounts.

Q5. Top 5 Categories with Highest Average Discount
SELECT category, ROUND(AVG(discountPercent),2)
FROM zepto
GROUP BY category
ORDER BY avg_discount DESC
LIMIT 5;

Q6. Price per Gram Analysis

Great for identifying best-value SKUs.

Q7. Product Weight Categorization (Low / Medium / Bulk)

Using CASE expressions.

Q8. Total Inventory Weight per Category

Useful for logistics and supply chain insights.

📘 Key Insights (Summary)

✔️ Some categories offer extremely high discounts, creating strong value propositions.
✔️ Several high-MRP items are out of stock, indicating strong demand and potential supply shortages.
✔️ Revenue varies significantly across categories based on stock & selling price.
✔️ Weight-based categorization helps understand product sizes popular in Zepto’s catalog.
✔️ Many items had inconsistent pricing units (paise), requiring conversion for accurate analysis.


🚀 How to Run This Project

Install PostgreSQL & pgAdmin

Create a new database (e.g., zepto_db)

Import CSV into the zepto table

Run the SQL queries from zepto_analysis.sql

View insights & results
