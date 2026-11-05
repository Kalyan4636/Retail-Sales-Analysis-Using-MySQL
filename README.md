# 🛒 Retail Sales Analysis | SQL Project

![SQL](https://img.shields.io/badge/SQL-MySQL%208.0%2B-blue)
![Level](https://img.shields.io/badge/Level-Beginner-success)
![Project](https://img.shields.io/badge/Project-Data%20Analytics-orange)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

## 📌 Project Overview

**Project Title:** Retail Sales Analysis  
**Database:** `p1_retail_db`  
**SQL Language:** MySQL 8.0+  
**Project Level:** Beginner  
**Domain:** Retail & E-commerce Analytics

The Retail Sales Analysis project demonstrates how SQL can be used to transform raw retail transaction data into meaningful business insights.

This project covers database creation, data cleaning, exploratory data analysis (EDA), and business-driven SQL queries to understand customer purchasing behaviour, product category performance, revenue trends, and transaction patterns.

It is designed for aspiring Data Analysts who want to strengthen their SQL fundamentals through practical, real-world business scenarios.

---

## 🎯 Project Objectives

1. **Database Creation:** Build a retail sales database using MySQL.
2. **Data Cleaning:** Identify missing values and maintain data quality.
3. **Exploratory Data Analysis:** Explore customer demographics, transactions, and product categories.
4. **Business Analysis:** Answer practical business questions using SQL.
5. **Insight Generation:** Identify sales patterns and customer purchasing behaviour.

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| MySQL 8.0+ | Database management |
| SQL | Data extraction and analysis |
| MySQL Workbench | SQL development environment |
| GitHub | Project documentation and version control |

### SQL Concepts Covered

- SELECT, WHERE, ORDER BY
- GROUP BY and HAVING
- Aggregate Functions (SUM, AVG, COUNT)
- DISTINCT and LIMIT
- Date and Time Functions
- CASE Statements
- Common Table Expressions (CTEs)
- Window Functions (RANK)
- Data Cleaning and NULL Handling

---

## 📂 Project Structure

```text
Retail-Sales-Analysis-SQL-Project/
│
├── README.md
│
├── database/
│   └── 01_database_setup.sql
│
├── sql/
│   ├── 02_data_cleaning.sql
│   ├── 03_exploratory_data_analysis.sql
│   └── 04_business_analysis.sql
│
├── data/
│   └── README.md
│
├── insights/
│   └── business_insights.md
│
├── screenshots/
│   └── README.md
│
└── .gitignore
```

---

## 🗄️ Database Setup

**Database Name:** `p1_retail_db`

The project uses a table called `retail_sales` with the following columns:

| Column Name | Description |
|---|---|
| transactions_id | Unique transaction ID |
| sale_date | Date of transaction |
| sale_time | Time of transaction |
| customer_id | Unique customer ID |
| gender | Customer gender |
| age | Customer age |
| category | Product category |
| quantity | Number of products sold |
| price_per_unit | Price of one unit |
| cogs | Cost of goods sold |
| total_sale | Total transaction amount |

### Create Database

```sql
CREATE DATABASE p1_retail_db;
USE p1_retail_db;
```

### Create Table

```sql
CREATE TABLE retail_sales (
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit DECIMAL(12,2),
    cogs DECIMAL(12,2),
    total_sale DECIMAL(12,2)
);
```

---

## 🧹 Data Cleaning & Exploratory Data Analysis

Before performing business analysis, the dataset is inspected for quality and consistency.

### Data Cleaning Tasks

- Identify missing and NULL values.
- Check incomplete transaction records.
- Validate customer and transaction information.
- Remove incomplete records when appropriate.

### Exploratory Data Analysis

- Calculate total transactions.
- Identify unique customers.
- Explore available product categories.
- Examine transaction date ranges.
- Calculate basic sales statistics.

---

## 📊 Business Questions & SQL Analysis

This project answers 10 practical business questions.

### 1. Retrieve all transactions made on 5 November 2022

```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

### 2. Find Clothing transactions with more than 4 units sold during November 2022

```sql
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
  AND sale_date >= '2022-11-01'
  AND sale_date < '2022-12-01'
  AND quantity > 4;
```

### 3. Calculate total sales and transactions for each category

```sql
SELECT
    category,
    SUM(total_sale) AS total_sales,
    COUNT(*) AS total_transactions
FROM retail_sales
GROUP BY category
ORDER BY total_sales DESC;
```

### 4. Calculate the average age of customers purchasing from the Beauty category

```sql
SELECT
    ROUND(AVG(age), 2) AS average_age,
    COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
WHERE category = 'Beauty';
```

### 5. Find transactions where total sales exceed 1,000

```sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000
ORDER BY total_sale DESC;
```

### 6. Calculate the number of transactions by gender in each category

```sql
SELECT
    category,
    gender,
    COUNT(*) AS total_transactions
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
```

### 7. Identify the month with the highest average transaction value in each year

```sql
WITH monthly_sales AS (
    SELECT
        YEAR(sale_date) AS sales_year,
        MONTH(sale_date) AS sales_month,
        AVG(total_sale) AS avg_sale
    FROM retail_sales
    GROUP BY YEAR(sale_date), MONTH(sale_date)
),
ranked_sales AS (
    SELECT *,
        RANK() OVER (
            PARTITION BY sales_year
            ORDER BY avg_sale DESC
        ) AS ranking
    FROM monthly_sales
)
SELECT
    sales_year,
    sales_month,
    ROUND(avg_sale, 2) AS average_sale
FROM ranked_sales
WHERE ranking = 1;
```

### 8. Find the top 5 customers based on total sales

```sql
SELECT
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

### 9. Calculate the number of unique customers in each category

```sql
SELECT
    category,
    COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;
```

### 10. Calculate the number of orders across Morning, Afternoon, and Evening shifts

```sql
WITH hourly_sales AS (
    SELECT *,
        CASE
            WHEN HOUR(sale_time) < 12
                THEN 'Morning'
            WHEN HOUR(sale_time) BETWEEN 12 AND 17
                THEN 'Afternoon'
            ELSE 'Evening'
        END AS sales_shift
    FROM retail_sales
)
SELECT
    sales_shift,
    COUNT(*) AS total_orders
FROM hourly_sales
GROUP BY sales_shift;
```

---

## 💡 Key Business Insights

The analysis is designed to uncover insights across six important business areas:

| Business Area | Analysis |
|---|---|
| Customer Demographics | Customer age and gender distribution |
| Category Performance | Revenue contribution by product category |
| High-Value Transactions | Transactions exceeding 1,000 |
| Monthly Sales Trends | Best-performing months by average sale |
| Customer Analysis | Top five customers by spending |
| Shopping Behaviour | Transaction distribution by time of day |

*Actual numerical findings should be added after executing the queries against the imported dataset.*

---

## 🚀 How to Run This Project

**Step 1: Clone the Repository**

```bash
git clone https://github.com/Kalyan4636/Retail-Sales-Analysis-SQL-Project.git
cd Retail-Sales-Analysis-SQL-Project
```

**Step 2: Open MySQL Workbench**

Connect to your MySQL server.

**Step 3: Create the Database**

Execute:

`database/01_database_setup.sql`

**Step 4: Import the Dataset**

Import your retail sales CSV file into the `retail_sales` table.

**Step 5: Perform Data Cleaning**

Execute:

`sql/02_data_cleaning.sql`

**Step 6: Perform Exploratory Data Analysis**

Execute:

`sql/03_exploratory_data_analysis.sql`

**Step 7: Run Business Analysis**

Execute:

`sql/04_business_analysis.sql`

Review the results and document your findings.

---

## 📈 Future Improvements

- Build an interactive Retail Sales Dashboard using Power BI.
- Perform customer segmentation using SQL.
- Analyse repeat purchases and customer retention.
- Identify revenue contribution by product category.
- Develop monthly and quarterly sales reports.
- Integrate Python for advanced data analysis.

---

## 👨‍💻 About the Author

**Aditya Kalyan**  
**Data Engineer at ORIX India | Data Analytics Trainer & Educator**

I am passionate about helping aspiring Data Analysts and Data Engineers develop practical technical skills through real-world projects, SQL challenges, and hands-on learning.

### Connect With Me

- **LinkedIn:** [Aditya Kalyan](https://www.linkedin.com/in/adityaakalyan/)
- **GitHub:** [Kalyan4636](https://github.com/Kalyan4636)
- **Portfolio:** [Data Analytics Portfolio](https://www.datascienceportfol.io/adityakalyanbscc)
- **Learning Community:** Data Detective Club

---

## 🌟 Support & Community

If you find this project useful:

- ⭐ Star this repository.
- 🍴 Fork the repository.
- 💬 Share your feedback and suggestions.
- 📢 Share it with aspiring Data Analysts.

### Join Data Detective Club (Learning Community)

Learn SQL, Excel, Power BI, Python, Data Analytics, and real-world project development.

**Investigate Data. Uncover Insights. Grow Together.**

---

## Created by Aditya Kalyan | Data Engineer | Data Analytics Trainer & Educator | Data Detective Club 
