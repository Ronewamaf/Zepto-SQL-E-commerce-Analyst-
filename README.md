# 🛒 Zepto-SQL-E-commerce-Analyst

A real-world SQL portfolio project that simulates advanced data analysis tasks using an inventory dataset from Zepto which is a fast-growing quick-commerce platform. This project mimics how Data Analysts work in the retail/e-commerce domain, performing database setup, data cleaning, exploration, and generating business insights with SQL.

---

## 📦 Dataset Overview

The dataset was scraped from Zepto’s official product listings and includes typical e-commerce inventory features. It was sourced from [Kaggle](https://www.kaggle.com/datasets) and reflects real-world catalog inconsistencies.

Each row represents a unique SKU (Stock Keeping Unit) for a product.

### 🔑 Columns:
| Column | Description |
|--------|-------------|
| `sku_id` | Unique product identifier (synthetic primary key) |
| `name` | Product name |
| `category` | Product category (e.g., Beverages, Fruits) |
| `mrp` | Maximum Retail Price (converted from paise to ₹) |
| `discountPercent` | Percentage discount applied |
| `discountedSellingPrice` | Final selling price (₹) |
| `availableQuantity` | Available inventory units |
| `weightInGms` | Product weight in grams |
| `outOfStock` | Boolean flag for stock availability |
| `quantity` | Units per package (mix of units and weight) |


---

## 🗃️ SQL Database Setup

```sql
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
````

---

## 📥 Data Import

I used the imort tool.
```

---

## 🔍 Exploratory Data Analysis (EDA)

```sql
-- Total Records
SELECT COUNT(*) FROM zepto;

-- Sample Rows
SELECT * FROM zepto LIMIT 10;

-- Null Value Check
SELECT 
  SUM(CASE WHEN mrp IS NULL THEN 1 ELSE 0 END) AS null_mrp,
  SUM(CASE WHEN discountedSellingPrice IS NULL THEN 1 ELSE 0 END) AS null_discountedSellingPrice,
  SUM(CASE WHEN category IS NULL THEN 1 ELSE 0 END) AS null_category
FROM zepto;

-- Unique Categories
SELECT DISTINCT category FROM zepto;

-- Out of Stock Analysis
SELECT outOfStock, COUNT(*) AS count FROM zepto GROUP BY outOfStock;

-- Duplicate Product Names
SELECT name, COUNT(*) FROM zepto GROUP BY name HAVING COUNT(*) > 1;
```

---

## 🧹 Data Cleaning

```sql
-- Remove rows where MRP or Selling Price = 0
DELETE FROM zepto WHERE mrp = 0 OR discountedSellingPrice = 0;

-- Convert Paise to Rupees (if needed)
-- Assuming original values were in paise (e.g., 10000 = ₹100)
UPDATE zepto
SET mrp = mrp / 100,
    discountedSellingPrice = discountedSellingPrice / 100
WHERE mrp > 1000;  -- Optional safeguard
```



## 📊 Business Insights

### 1. Top 10 Best Value Products (by Discount %)

```sql
SELECT name, mrp, discountedSellingPrice, discountPercent
FROM zepto
ORDER BY discountPercent DESC
LIMIT 10;
```

### 2. High-Value Products Out of Stock

```sql
SELECT name, category, mrp
FROM zepto
WHERE outOfStock = TRUE AND mrp > 500
ORDER BY mrp DESC;
```

### 3. Estimated Revenue by Category

```sql
SELECT category,
       SUM(discountedSellingPrice * availableQuantity) AS estimated_revenue
FROM zepto
GROUP BY category
ORDER BY estimated_revenue DESC;
```

### 4. Categories with Highest Average Discount

```sql
SELECT category,
       AVG(discountPercent) AS avg_discount
FROM zepto
GROUP BY category
ORDER BY avg_discount DESC
LIMIT 5;
```

### 5. Price per Gram for Value Products

```sql
SELECT name, discountedSellingPrice, weightInGms,
       ROUND(discountedSellingPrice / weightInGms, 2) AS price_per_gram
FROM zepto
WHERE weightInGms IS NOT NULL AND weightInGms > 0
ORDER BY price_per_gram ASC
LIMIT 10;
```

### 6. Grouping Products by Weight Category

```sql
SELECT *,
  CASE 
    WHEN weightInGms <= 500 THEN 'Low'
    WHEN weightInGms <= 2000 THEN 'Medium'
    ELSE 'Bulk'
  END AS weight_category
FROM zepto;
```

### 7. Inventory Weight by Category

```sql
SELECT category,
       SUM(weightInGms * availableQuantity) AS total_weight
FROM zepto
GROUP BY category
ORDER BY total_weight DESC;
```

---

## ✅ Key Learnings

* Practical SQL for e-commerce analytics
* Managing messy real-world data
* Applying business logic using SQL
* Data cleaning strategies in PostgreSQL
* Transforming raw data into actionable insights

---

## 🛠 Tech Stack

* PostgreSQL
* SQL
* pgAdmin
* CSV File Handling
* EDA & BI logic


--Let me know if you'd like me to customize this further with your name, LinkedIn, or GitHub handle!
```
