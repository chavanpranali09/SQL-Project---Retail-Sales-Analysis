# SQL-Project---Retail-Sales-Analysis
This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries.

-- Database Creation
'''sql
create database sql_project;
use sql_project;
'''

-- Table Creation
CREATE TABLE retail_sale (
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(15),
    quantity INT,
    price_per_unit INT,
    cogs FLOAT,
    total_sale INT
);
    
-- Import Dataset and Data Exploration:
select * from retail_sale;

-- Cleaning the data
select * from retail_sale
where transactions_id is null;

-- Check Null Value
SELECT * FROM
    retail_sale
WHERE
    transactions_id IS NULL
        OR sale_date IS NULL
        OR sale_time IS NULL
        OR customer_id IS NULL
        OR gender IS NULL
        OR age IS NULL
        OR category IS NULL
        OR quantity IS NULL
        OR price_per_unit IS NULL
        OR cogs IS NULL
        OR total_sale IS NULL;

-- Record Count
select count(*) from retail_sale;

-- How many sales we have?
select count(*) as total_sale from retail_sale;

-- How many unique customers we have?
select count(distinct customer_id) from retail_sale;

-- How many categories we have?
select distinct category from retail_sale;

-- Data Analysis & Finding Business Problems & Answers

-- Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05
-- Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 10 in the month of Nov-2022
-- Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.
-- Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.
-- Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.
-- Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
-- Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year
-- Q.8 Write a SQL query to find the top 5 customers based on the highest total sales 
-- Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.
-- Q.10 Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)


-- Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05'.
SELECT * FROM retail_sale
WHERE
    sale_date = '2022-11-05';

-- Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 3 in the month of Nov-2022.
SELECT *
FROM retail_sale
WHERE
    category = 'clothing' AND quantity > 3
        AND MONTH(sale_date) = 11
        AND YEAR(sale_date) = 2022;

-- Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.
SELECT 
    category,
    SUM(total_sale) AS total_sale,
    COUNT(*) AS total_order
FROM
    retail_sale
GROUP BY category;

-- Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.
SELECT 
    ROUND(AVG(age), 2) AS Avg_Age
FROM
    retail_sale
WHERE
    category = 'Beauty';

-- Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.
SELECT *
FROM retail_sale
WHERE
    total_sale > 1000;

-- Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
SELECT 
    category, gender, COUNT(*) AS total_transactions
FROM
    retail_sale
GROUP BY category , gender
ORDER BY 1;

-- Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year.
SELECT Date, Month, total_sale FROM
(SELECT YEAR(sale_date) AS Date, Month(sale_date) AS Month, AVG(total_sale) AS total_sale,
RANK() OVER (PARTITION BY (YEAR(sale_date)) ORDER BY AVG(total_sale) DESC) AS top
FROM retail_sale
GROUP BY YEAR(sale_date), Month(sale_date)) AS t1
WHERE top = 1;

-- Q.8 Write a SQL query to find the top 5 customers based on the highest total sales
SELECT 
    customer_id, SUM(total_sale) AS total_sale
FROM
    retail_sale
GROUP BY customer_id
ORDER BY 2 DESC
LIMIT 5;

-- Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.
SELECT 
    category, COUNT(DISTINCT customer_id) AS unique_customers
FROM
    retail_sale
GROUP BY category;
 
-- Q.10 Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)
SELECT shift, COUNT(*) AS number_of_orders
FROM
    (SELECT *,
            CASE
                WHEN TIME(sale_time) <= '12:00:00' THEN 'Morning_Shift'
                WHEN TIME(sale_time) > '12:00:00' AND TIME(sale_time) <= '17:00:00'
                THEN 'Afternoon_Shift'
                ELSE 'Evening_Shift'
            END AS Shift
    FROM retail_sale) AS t1
GROUP BY shift
ORDER BY FIELD(Shift,
        'Morning_Shift',
        'Afternoon_Shift',
        'Evening_Shift');
        
-- End 










    
