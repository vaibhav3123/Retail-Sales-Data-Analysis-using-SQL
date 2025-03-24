**🛍️ Retail Sales Analysis SQL Project**

This project demonstrates SQL techniques used for analyzing retail sales data. It involves setting up a database, performing exploratory data analysis (EDA), and answering key business questions through SQL queries.


**🎯 Objectives**

**🗄️ Database Setup:** Create and populate a retail sales database.

**🧹 Data Cleaning:** Identify and remove records with missing values.

**📊 Exploratory Data Analysis:** Gain insights by exploring the dataset.

**💡 Business Analysis:** Use SQL queries to derive meaningful insights.

---

**📌 Project Structure**

### 1️⃣ Database Setup

```sql
CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales (
    transaction_id INT PRIMARY KEY,
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

---

### 2️⃣ Data Exploration & Cleaning

**🔍 Basic Data Exploration**

```sql
-- 📊 Total records in dataset
SELECT COUNT(*) FROM retail_sales;

-- 🔢 Count of unique customers
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;

-- 🏷️ Unique product categories
SELECT DISTINCT category FROM retail_sales;
```

**🧼 Null Value Check & Data Cleaning**

```sql
-- ❓ Checking for NULL values
SELECT * FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL
    OR gender IS NULL OR age IS NULL OR category IS NULL
    OR quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

-- 🗑️ Removing records with NULL values
DELETE FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL
    OR gender IS NULL OR age IS NULL OR category IS NULL
    OR quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

---

### 3️⃣ Data Analysis & Insights


**📊 Key Business Questions Answered Using SQL**

1️⃣ **Sales on a Specific Date ('2022-11-05')**

```sql
SELECT * FROM retail_sales WHERE sale_date = '2022-11-05';
```

2️⃣ **Clothing Sales (Quantity > 4) in November 2022**

```sql
SELECT * FROM retail_sales
WHERE category = 'Clothing'
AND sale_date BETWEEN '2022-11-01' AND '2022-11-30'
AND quantity >= 4;
```

3️⃣ **Total Sales by Product Category**

```sql
SELECT category, SUM(total_sale) AS net_sale, COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
```

4️⃣ **Average Age of Customers Purchasing from 'Beauty' Category**

```sql
SELECT ROUND(AVG(age), 2) AS avg_age FROM retail_sales WHERE category = 'Beauty';
```

5️⃣ **High-Value Transactions (Total Sale > 1000)**

```sql
SELECT * FROM retail_sales WHERE total_sale > 1000;
```

6️⃣ **Transactions by Gender in Each Category**

```sql
SELECT category, gender, COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
```

7️⃣ **Best-Selling Month Each Year**

```sql
SELECT year, month, avg_sale FROM (
    SELECT EXTRACT(YEAR FROM sale_date) AS year, EXTRACT(MONTH FROM sale_date) AS month,
           AVG(total_sale) AS avg_sale,
           RANK() OVER (PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales
    GROUP BY year, month
) AS ranked_sales
WHERE rank = 1;
```

8️⃣ **Top 5 Customers by Total Sales**

```sql
SELECT customer_id, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

9️⃣ **Unique Customers per Category**

```sql
SELECT category, COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;
```

🔟 **Sales Distribution by Shift (Morning, Afternoon, Evening)**

```sql
WITH hourly_sales AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders
FROM hourly_sales
GROUP BY shift
ORDER BY total_orders DESC;
```

---

### **📌 Findings & Recommendations**

- **Customer Demographics:** Sales are distributed across various age groups and product categories.
- **High-Value Transactions:** Several purchases exceed $1000, highlighting premium products.
- **Sales Trends:** Identified best-selling months and seasonal consumer trends.
- **Customer Insights:** Determined top-spending customers and popular product categories.

**✅ Recommendations:**

- **Inventory Management:** Stock up on high-demand products in peak sales months.
- **Targeted Marketing:** Focus promotions on high-value customers and top-selling categories.
- **Shift Optimization:** Allocate resources based on peak sales times.
- **Seasonal Promotions:** Introduce discounts during high-sales periods.

---

### **🔧 How to Use**

1️⃣ Load the dataset into a PostgreSQL database.

2️⃣ Run the SQL queries to explore and analyze the data.

3️⃣ Modify or extend queries based on your analysis needs.


### **🛠️ Tools Used**

- PostgreSQL for SQL queries.
- pgAdmin for query execution and database management.

---

### **📬 Contact & Contributions**

For any questions, discussions, or improvements, feel free to reach out:

🔗 **GitHub:** [https://github.com/vaibhav3123](https://github.com/vaibhav3123)  
💼 **LinkedIn:** [https://www.linkedin.com/in/vaibhav-bari-915bb5202/](https://www.linkedin.com/in/vaibhav-bari-915bb5202/)  
📧 **Email:** bariv219@gmail.com  

🚀 **Happy Analyzing!**

