# SQL_reatil_sales_project

## Project Overview

##Project Title: Retail Sales Analysis
##Databases: 'sql-project-1'

This project is designed to demostrate SQL skills and techniques typically used by data analysts to explore ,clean and analyze retail sales data.The project involves setting up a retail sales database,performing exploratory data analysis (EDA), and answering specific business question through SQL queries .

## Objectives 

1.**Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2.##Data Cleaning** : Identify and remove any records with missing or null values.
3.**Exploratory Data Analysis (EDA)**:Perform basic exploratory data analyis to understand the dataset.
4.**Business Analysis**: Use SQL to answer specific business question and derive from the sales data.

## Project- Structure

### 1. Database Setup

-**Database Creation**: The project starts by creating a database names 'sql-project-1'.
-**Table Creation** : A table  named 'retail_sales' is created to store the sales data. The table structure includes columns for transaction ID, sale date,sale time,customer ID,gender,age,product category,quantity sold, price per unit,cost of goods sold(COGS),and total sale amount.

```sql
CREATE DATABASE sql-project-1;


CREATE TABLE retail_sales
( 
       transactions_id INT,
				sale_date	DATE,
				sale_time	TIME,
				customer_id  INT,
				gender	    
				age
				category
				quantiy	
				price_per_unit
				cogs
				total_sale
	);
  ```

  ### 2. Data Exploration & Cleaning

  -**Record Count**: Determine the total number of records in the dataset.
  -**Customer Count**: Find out how many unique customers are in the dataset.
  -**Category Count** Identify all unique products categories  in the dataset
  -**Null Value Check**: Check for any null values in the dataset and delete records with missing data.

  ```sql
  SELECT COUNT(*) FROM retail_sales;
  SELECT COUNT(DISTINCT category) FROM retail_sales;
  SELECT COUNT(DISTINCT category) FROM retail_sales;

  SELECT * FROM retail_sales
  WHERE
     sale date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR gender IS NULL OR age IS        NULL OR category IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
 ```

 ### 3. Data Analysis & Findings
The following SQL queries were developed to answer specific business questions:

1. **Write a sql query to retrive all columns for sales made on '2022-11-05'**:
  ```sql
    SELECT * FROM retail_sales
    WHERE sale_date='2022-11-05';
  ```
2. **Write a sql query to retrive all transaction where the category is 'Clothing' and the qauntity sold is more than 4 in the month of nov-2022**
  ```sql
    SELECT  *
    FROM retail_sales
    WHERE
        category ='Clothing'
        AND sale_date>='2022-11-01' AND sale_date<'2022-12-01'
        AND quantiy>=4;
  ```
3. ** Write a sql query to calculate the total sale for each category**:
   ```sql
      SELECT category,
      SUM(total_sale) as net_sale
      FROM retail_sales
      GROUP BY category;
    ```
4. **Write a sql query to find the average age of customer who purchased item from the 'Beauty' category**:
    ```sql
        SELECT  ROUND(AVG(age),2) as avg_age
        FROM retail_sales
        WHERE category='Beauty'
     ```
5. **Write a sql query to find all transaction where the total_sale is greater than 1000**:
   ```sql
      SELECT * FROM retail_sales
      WHERE total_sale>1000
   ```
6. **Write a sql quey to find the total number of transactions (transaction_id) made by each    gender in each category**
   ```sql
       SELECT 
          gender,
          category,
          COUNT(*) AS total_transactions
       FROM retail_sales
       GROUP 
           BY gender,
           category
       ORDER BY category
   ```
7. **Write a sql query to calculate the avgerage sales for each month find out best selling month in each year**:
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
         RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date ) ORDER BY  AVG(total_sale) DESC) as rank
      FROM retail_sales
      GROUP BY year,month 
      ) as t1
      WHERE t1 .rank  =  1;
   ```
8.**Write a sql query to find the top 5 customers based on the highest total sales**:
  ```sql
     SELECT
        customer_id,
         SUM(total_sale) as total_sales
     FROM retail_sales
     GROUP BY customer_id
     ORDER BY total_sales DESC
     LIMIT 5;
  ```
9.** Write a sql query to find the number of unique customers who purchased items from each category**:
  ```sql
    SELECT
        category,
    COUNT(DISTINCT customer_id)
    FROM retail_sales
    GROUP BY category
  ```
10.**Write a sql query to create each shift and number of orders(Example morning <12, afternoon between 12 and 17, evening >17)**
   ```sql
      WITH hourly_sale as
      (
       SELECT *,
           CASE 
                WHEN EXTRACT(HOUR FROM sale_time)<12 THEN 'Morning'
              	WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
               	ELSE 'Evening'
          END as shift
        FROM retail_sales
        )
        SELECT
           shift,
           COUNT(*) as total_orders
        FROM hourly_sale
        GROUP BY shift
   ```
## Findings
-**Customer Demographics: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty and Electronics.
-**High-Value Transactions: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
-**Sales Trends: Monthly analysis shows variations in sales, helping identify peak seasons.
-**Customer Insights: The analysis identifies the top-spending customers and the most popular product categories.


## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.




