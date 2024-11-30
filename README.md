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
-	Objective: Analyze yearly revenue trends to identify growth patterns.
-	Findings:
o	To analyze monthly revenue trends, SQL was utilized to calculate total revenue for orders with a status of 'Complete.' The query grouped transactions by month and summed their corresponding sales. 
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
o	Visualization : A line chart showcasing yearly revenue growth revealed peak periods.
