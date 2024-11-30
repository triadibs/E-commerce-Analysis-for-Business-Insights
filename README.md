# E-commerce-Analysis-for-Business-Insights
## Project Overview
This project explores an e-commerce dataset, "The Look E-commerce," containing transactional, product, and user information. The objectives were to evaluate key performance metrics such as revenue trends, product performance, and market segmentation based on demographic factors.
Dataset Description
Dataset Description
The dataset used for this analysis is the The Look E-Commerce Dataset, a publicly available dataset hosted on Google BigQuery Public Datasets. This dataset provides a detailed view of the operations of an e-commerce platform, including transactions, product information, and customer demographics. It is structured across three interrelated tables:
1.	Order Items:
- Columns: order_id, user_id, product_id, status, created_at, shipped_at, sale_price, and others.
- Contains details of individual orders, including transaction status and timestamps.
2.	Products:
-	Columns: id, category, brand, retail_price, cost, and others.
-	Stores product-related information such as category, price, and brand.
3.	Users:
-	Columns: id, age, gender, traffic_source, created_at, and others.
-	Represents customer profiles with demographic and traffic source data.
## Key Analysis and Insights
### 1. Revenue Trends
#### -	Objective: Analyze yearly revenue trends to identify growth patterns.
#### -	Findings:
-	To analyze monthly revenue trends, SQL was utilized to calculate total revenue for orders with a status of 'Complete.' The query grouped transactions by month and summed their corresponding sales. 
The SQL query used for this analysis is as follows:

```
SELECT 
    FORMAT_TIMESTAMP('%Y-%m', TIMESTAMP(created_at)) AS month_year,
    ROUND(SUM(sale_price), 2) AS total_revenue
FROM 
    `bigquery-public-data.thelook_ecommerce.order_items`
WHERE 
    status = 'Complete'
GROUP BY 
    month_year
ORDER BY 
    month_year;
```
-	Visualization : A line chart showcasing yearly revenue growth revealed peak periods.
  ![total revenue by year](https://github.com/user-attachments/assets/45677b19-2011-4ac3-9dd7-ebb1b89e8728)
### 2. Best-Selling Products
#### •	Objective: Identify top-performing products to optimize inventory and marketing strategies.
#### •	Findings:
-	The most frequently sold products were identified by calculating the total sales volume. 
The SQL query used to derive these insights is as follows:
```
SELECT 
    p.name AS product_name,
    COUNT(oi.product_id) AS quantity_sold,
    ROUND(SUM(oi.sale_price), 2) AS total_revenue
FROM 
    bigquery-public-data.thelook_ecommerce.order_items oi
JOIN 
    bigquery-public-data.thelook_ecommerce.products p
ON 
    oi.product_id = p.id
WHERE 
    oi.status = 'Complete'
GROUP BY 
    product_name
ORDER BY 
    quantity_sold DESC
    limit 10;
```
-	Visualization: A bar chart of the top 10 products by sales highlighted key drivers of product success.
  ![top 10 products](https://github.com/user-attachments/assets/ca1a4484-66eb-405b-a4d5-277ca4a6e18d)
### 3. Market Segmentation
#### •	Objective: Determine the largest customer segment based on age and gender to target marketing efforts effectively.
#### •	Findings:
-	Gender: Transactions were fairly balanced between male and female customers, indicating an equal level of engagement across genders.
-	Age Groups: Customers aged 55+ contributed the highest revenue, suggesting they represent a crucial market segment with significant purchasing power.
The SQL query used to analyze these demographics is provided below:
```
SELECT 
    u.gender,
    CASE 
        WHEN u.age BETWEEN 18 AND 24 THEN '18-24'
        WHEN u.age BETWEEN 25 AND 34 THEN '25-34'
        WHEN u.age BETWEEN 35 AND 44 THEN '35-44'
        WHEN u.age BETWEEN 45 AND 54 THEN '45-54'
        ELSE '55+'
    END AS age_group,
    ROUND(SUM(oi.sale_price), 2) AS total_revenue
FROM 
    bigquery-public-data.thelook_ecommerce.order_items oi
JOIN 
    bigquery-public-data.thelook_ecommerce.users u
ON 
    oi.user_id = u.id
WHERE 
    oi.status = 'Complete'
GROUP BY 
    u.gender, age_group
ORDER BY 
    total_revenue DESC;
```
- Visualization: A segmented bar chart showing gender-based contributions and age. 
![Market Analysis by Gender and Age](https://github.com/user-attachments/assets/d2f6bcd9-470a-4cf1-8be2-d965f860c769)



