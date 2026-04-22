<h1 align="center">🛍️ Retail Sales Analysis (SQL Project)</h1>

<p align="center">
  <b>End-to-End SQL Data Analysis Project | Beginner-Friendly Portfolio</b>
</p>

---

## 📌 Project Overview

This project demonstrates how SQL is used in real-world data analysis to explore, clean, and analyze retail sales data.

It follows a complete data analyst workflow:
- 🧱 Database Setup  
- 🧹 Data Cleaning  
- 📊 Exploratory Data Analysis (EDA)  
- 📈 Business Problem Solving  

---

## 🎯 Objectives

- Build a structured retail database  
- Clean and validate raw data  
- Perform exploratory analysis  
- Solve business-driven SQL queries  

---

## 🗂️ Dataset Description

| Column            | Description |
|------------------|------------|
| Transaction ID    | Unique sale identifier |
| Sale Date/Time    | Date & time of transaction |
| Customer ID       | Unique customer identifier |
| Gender            | Customer gender |
| Age               | Customer age |
| Category          | Product category |
| Quantity          | Items purchased |
| Price per Unit    | Unit price |
| COGS              | Cost of goods sold |
| Total Sale        | Final transaction value |

---

## 🛠️ Tools & Technologies

- 🧠 SQL (PostgreSQL)
- 🧹 Data Cleaning Techniques
- 📊 Aggregation & Filtering
- ⚙️ Window Functions

---

## 🧩 Database Setup

```sql
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
```

## 📊 Exploratory Data Analysis (EDA)

This stage focuses on understanding the structure and quality of the dataset before performing business analysis.

- 📦 Analyzed total number of transactions in the dataset  
- 👥 Identified unique customers and their distribution  
- 🏷️ Explored product categories and their frequency  
- 📈 Evaluated overall sales patterns and dataset structure  
- 🔍 Verified data completeness and handled missing values
  
```sql
SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL; 

```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL Query to retrieve all columns for sales made on '2022-11-05'**:
```sql
SELECT
  *
FROM retail_sales 
WHERE
    sale_date = '2022-11-05'
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
SELECT 
  *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
SELECT 
    category,
    SUM(total_sale) as net_sale
FROM retail_sales
GROUP BY category;
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
SELECT
    category,
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty'
GROUP BY category
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
SELECT 
    category,
    gender,
    COUNT(*) as total_trans_per_car_per_gend
FROM retail_sales
GROUP 
    BY 
    category,
    gender
ORDER BY category
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
SELECT 
       year,
       month,
    avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales
GROUP BY year, month
) as t1
WHERE rank = 1
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
```sql
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
SELECT
    COUNT(*) AS total_orders,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END AS shift
FROM retail_sales
GROUP BY 
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END;

```

## 📈 Key Insights

- 📊 Sales are distributed across multiple product categories, with **Clothing and Beauty** contributing significantly  
- 💰 Presence of high-value transactions (above 1000) indicates **premium customer segments**  
- 📅 Monthly analysis reveals **seasonal trends**, helping identify peak sales periods  
- 👥 Customer behavior varies across **gender and product categories**  
- 🏆 A small group of customers contributes to a **large portion of total revenue** (top 5 customers)  
- ⏰ Sales activity varies by time of day, with distinct **morning, afternoon, and evening patterns**  

---

## 💼 Business Impact

This analysis provides valuable insights that can support business decision-making:

- 🎯 Helps identify **top-performing product categories** for better inventory planning  
- 👥 Enables **customer segmentation** for targeted marketing strategies  
- 📈 Supports **sales trend analysis** for forecasting and planning  
- 💡 Identifies **high-value customers** for loyalty programs and retention  
- ⏱️ Assists in optimizing **store operations based on peak sales hours**  

---

## 🚀 Conclusion

This project demonstrates the practical application of SQL in real-world data analysis scenarios.  
It covers the complete data analysis workflow — from **data cleaning and exploration to business insight generation**.

Through this project, key analytical skills were applied, including:
- Data validation and cleaning  
- Aggregation and filtering  
- Window functions  
- Business-oriented query design  

Overall, the project highlights how raw retail data can be transformed into **actionable insights** that drive smarter business decisions.

---

## 👩‍💻 Author

**Saiqa**  
🎓 Software Engineering Student  
📊 Aspiring Data Analyst  

---

## ⭐ Support

If you found this project helpful or interesting:

- ⭐ Star the repository  
- 🍴 Fork it and try your own analysis  
- 💬 Share feedback or suggestions  

---
