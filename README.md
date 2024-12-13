# E-commerce-Analysis-for-Business-Insights
## Project Overview
This project offers a comprehensive analysis of The Look e-commerce dataset to uncover actionable insights on customer behavior, product performance, and overall business operations. By exploring detailed transaction data, customer demographics, and product attributes, we aim to identify trends, patterns, and growth opportunities critical to e-commerce success.
Our analysis focuses on key performance indicators such as monthly revenue trends, customer retention, product return rates, and category performance by demographic segments. Leveraging advanced analytics techniques, the project identifies drivers of success and provides data-driven recommendations to enhance strategic decision-making and improve overall efficiency.

## Background/Context
In the highly competitive world of e-commerce, understanding customer behavior, product performance, and operational efficiency is crucial for maintaining growth and profitability. With rapid digitalization and ever-changing consumer preferences, businesses are now more reliant on data-driven decision-making to remain agile and competitive.

The Look E-commerce operates in a dynamic market where success is driven by the ability to adapt to customer needs, optimize product offerings, and manage operational challenges such as cancellations and returns. The data generated from daily transactions, customer demographics, and product interactions offer valuable opportunities for insights, but without proper analysis, these opportunities remain untapped.

This project aims to analyze a comprehensive dataset from The Look E-commerce containing details of transactions, customer profiles, and product attributes. The goal is to identify key trends, uncover actionable insights, and provide strategic recommendations that enhance customer satisfaction, streamline operations, and maximize profitability.

Through this analysis, we will address critical questions such as:
- What are the monthly sales trends, and how do cancellations impact revenue?
- Which product categories perform best, and how do they vary by customer demographics?
- Which categories are most likely to be returned, and what factors contribute to this behavior?
The outcomes of this study will not only provide clarity on past performance but also pave the way for informed strategic planning and improved business outcomes.

## Data Collection and Preparation
The dataset used for this project is sourced from the Google BigQuery Public Data, specifically the "The Look E-commerce" dataset. This dataset captures detailed information about transactions, customers, and products in an e-commerce setting. Below is a description of the key columns:
1.	id: A unique identifier for each transaction.
2.	order_id: The identifier for a specific order, which may include multiple products.
3.	user_id: The unique identifier for a customer.
4.	product_id: The identifier for a product in the catalog.
5.	status: The current status of the order (e.g., completed, refunded, returned).
6.	created_at: The timestamp indicating when the order was created.
7.	sale_price: The price at which the product was sold.
8.	gender: The gender of the customer (e.g., male, female, other).
9.	num_of_item: The quantity of items purchased in the order.
10.	cost: The cost of the product to the business.
11.	category: The category to which the product belongs (e.g., electronics, apparel).
12.	name: The name of the product.
13.	brand: The brand of the product.
14.	age: The age of the customer.
15.	state: The geographical state of the customer.
This dataset provides a robust foundation for analyzing various aspects of e-commerce performance, including customer behavior, product trends, and financial metrics.
![datasaet info](https://github.com/user-attachments/assets/728ee8ec-a3c2-4f73-940e-9d5fdcd8b76c)
![flow](https://github.com/user-attachments/assets/2d54857b-9b16-4789-8327-1513099faafa)
```
SELECT 
    -- Mengubah format created_at menjadi DATE
    DATE(oi.created_at) AS created_at_date,

    -- Membulatkan sale_price dan cost
    CAST(ROUND(oi.sale_price, 2) AS NUMERIC) AS sale_price_rounded,
    CAST(ROUND(p.cost, 2) AS NUMERIC) AS cost_rounded,

    -- Menambahkan kolom revenue
    (oi.sale_price * o.num_of_item) - (p.cost * o.num_of_item) AS revenue,

    -- Menambahkan kolom age_group
    CASE
        WHEN u.age < 18 THEN 'Under 18'
        WHEN u.age BETWEEN 18 AND 24 THEN '18-24'
        WHEN u.age BETWEEN 25 AND 34 THEN '25-34'
        WHEN u.age BETWEEN 35 AND 44 THEN '35-44'
        WHEN u.age BETWEEN 45 AND 54 THEN '45-54'
        WHEN u.age >= 55 THEN '55+'
        ELSE 'Unknown'
    END AS age_group,

    
    oi.id,
    oi.order_id,
    oi.user_id,
    oi.product_id,
    oi.status,
    oi.sale_price,
    o.gender,
    o.num_of_item,
    p.cost,
    p.category,
    p.name,
    p.brand,
    u.age,
    u.country

FROM `bigquery-public-data.thelook_ecommerce.order_items` oi
LEFT JOIN `bigquery-public-data.thelook_ecommerce.orders` o
    ON o.order_id = oi.order_id
LEFT JOIN `bigquery-public-data.thelook_ecommerce.products` p
    ON p.id = oi.product_id
LEFT JOIN `bigquery-public-data.thelook_ecommerce.users` u
    ON u.id = oi.user_id;
```
### Changes Implemented:
1.	Conversion of created_at to created_at_date:
o	The created_at column is transformed into a DATE type using the DATE() function for better handling of date-specific analysis.
2.	Addition of revenue Column:
o	A new column, revenue, is added, calculated using the formula: Revenue=(sale_price×num_of_item)−(cost×num_of_item)\text{Revenue} = (\text{sale\_price} \times \text{num\_of\_item}) - (\text{cost} \times \text{num\_of\_item})Revenue=(sale_price×num_of_item)−(cost×num_of_item) This column helps analyze profit margins at a granular level.
3.	Creation of age_group Column:
o	A new age_group column is introduced to classify customers into predefined age categories using a CASE statement. This categorization aids in demographic segmentation and targeted insights.

## Exploratory Data Analysis (EDA)
### Trend Revenue by Year
![Revenue Trend Over Time_Page_1](https://github.com/user-attachments/assets/eb2bdb01-4342-4585-88e3-ff9e0a11d239)
The revenue trend analysis from 2019 to 2024 showcases a remarkable growth trajectory. Starting with a modest $0.07M in 2019, revenue grew exponentially by 257% to $0.25M in 2020. Although the YoY growth moderated in subsequent years, it remained robust at 63% in 2022 and 58.67% in 2023.
In 2024, revenue surged by 100% YoY, reaching a peak of $2.38M. This dramatic increase likely reflects successful product innovations or significant market expansions. The trend indicates a strong upward trajectory, suggesting sustainable business growth.

### Trend Complete, Cancelled, Returned
![thelook_Page_2](https://github.com/user-attachments/assets/7f252f81-84ea-4f52-b2dd-25a1fcf82b9c)
The analysis of order status trends from 2019 to 2024 reveals consistent improvements in operational performance:
-	The completion rate increased steadily, growing from 48.9% in 2019 to 50.4% in 2024, showcasing improved delivery and order fulfillment processes.
-	The cancellation rate fluctuated slightly but remained relatively stable, ranging between 29.37% and 30.1%.
-	The return rate has shown a clear downward trend, decreasing from 21% in 2019 to 19.86% in 2024, likely indicating enhanced customer satisfaction with product quality and reduced dissatisfaction-driven returns.
-	This overall improvement highlights the company’s sustained focus on optimizing operations and customer satisfaction.

### CUSTOMER DEMOGRAPHIC
![top 5 country_Page_1](https://github.com/user-attachments/assets/4f5642f1-0223-4ea9-8f41-ba651d421f17)
China leads as the top customer demographic, followed by the United States in second place and Brazil in third.
![top 5 country_Page_2](https://github.com/user-attachments/assets/836b69be-2ae4-479e-b071-69a75ef7ca01)
The majority of customers fall within the 25-64 age range, followed by the teenage group aged 12-24. Senior customers aged 64-75 are present but represent a smaller portion. These age groups show equal participation from both genders.

### Most Returned Brand and Category
![most profitable brand_Page_4](https://github.com/user-attachments/assets/750584d4-5699-467b-9fd6-9f8860eac895)

### Decomposition Tree
![decomposition tree_Page_1](https://github.com/user-attachments/assets/d3a1e731-0450-4d24-a8cf-ed913086e139)
![decomposition tree_Page_2](https://github.com/user-attachments/assets/2a96e60d-fb3b-4f12-953d-18b3b0b2bd38)
![decomposition tree_Page_3](https://github.com/user-attachments/assets/554143e0-0d51-4cb0-acd8-fc378bb688ff)

## Metrics Calculation and Analysis
### Average Order Value 2024
![AOV_Page_1](https://github.com/user-attachments/assets/969a4569-50aa-4606-9fd2-e73771e0fcf7)
#### AOV vs. MAU
Observations:
1.	AOV Trends:
-	Relatively stable across months with slight peaks and drops.
-	Highest AOV in April (166) does not correspond to the highest MAU.
-	AOV shows a slight decline in the latter months, particularly October (155) and November (158).
2.	MAU Trends:
-	Consistently increasing throughout the year, peaking in November (7,241).
-	The most significant growth in MAU occurs from September to November, a period where AOV slightly decreases.
  
Key Insights:
-	An increase in MAU, particularly in the later months, might bring in more casual or first-time buyers who make lower-value purchases, leading to a slight dip in AOV.
-	To address this, strategies to increase AOV among new users (e.g., targeted promotions, product bundling) can help maximize revenue from the growing customer base.

### Cohort Retention Rate in 2024
![cohort chart](https://github.com/user-attachments/assets/7b0aec87-258e-4744-a768-6acf331f6070)
#### Cohort Analysis Overview
#### Overview
The cohort chart tracks retention rates (%) across months, where month 1 represents the acquisition month and subsequent months represent the retention percentage of the original cohort. Below is a detailed analysis:
#### 1. Retention Trends
-	Gradual Decline:
Retention generally decreases as time progresses, which is expected. Earlier cohorts (January–June) exhibit a more balanced decline in retention compared to later cohorts.
*	Peak Retention Periods:
-	July and August cohorts stand out with higher retention rates in months 2–4, maintaining above 7%, suggesting better engagement strategies during those periods.
-	The June cohort has the highest Month 2 retention rate (8.81%) among early cohorts.
#### 2. Anomalies in Retention
*	September and October Cohorts:
-	The September cohort has an exceptionally high retention rate in Month 2 (12.52%), far exceeding earlier cohorts. However, this retention sharply declines in later months (9.67% in Month 3, with no data for subsequent months).
-	Similarly, the October cohort exhibits the highest initial retention rate (14.58%) but lacks data beyond Month 2.
*	November Cohort:
No retention data beyond Month 1, as it is still within the acquisition period.
*	The December cohort cannot be fully analyzed due to the short time frame (data only reflects a few days).
Interpretation:
The September and October cohorts likely benefited from strong acquisition campaigns, possibly tied to specific marketing efforts or seasonal promotions. However, the retention decline suggests a need for follow-up engagement strategies to sustain user activity.
#### 3. Patterns of Retention Decline
*	Early Cohorts (January–June):
-	The drop-off is relatively moderate, with retention stabilizing around 4–5% in later months.
-	This stability indicates effective retention strategies but also highlights potential for further improvement in user engagement.
*	Later Cohorts (July–October):
-	Higher initial retention rates (8–14%) suggest improvements in onboarding or engagement strategies during these periods.
#### Key Insights & Recommendations
1.	Sustain Engagement for High-Performing Cohorts:
-	Leverage insights from July–October cohorts to understand the factors driving their high initial retention (e.g., onboarding processes, targeted campaigns).
-	Implement strategies to maintain retention beyond Month 3, such as loyalty programs, reactivation emails, or exclusive offers.
2.	Focus on Long-Term Retention:
-	For earlier cohorts (e.g., January–June), where retention stabilizes at 4–5%, consider enhancing re-engagement campaigns to boost long-term activity.
3.	Address Retention Drop-Off in Later Cohorts:
-	September and October cohorts show steep declines after Month 2. Focus on personalized engagement strategies during the critical second and third months to minimize drop-off.
4.	Evaluate Marketing Efforts:
-	The surge in Month 1 retention for later cohorts (e.g., October with 14.58%) indicates successful acquisition campaigns. Replicate these efforts while pairing them with sustained post-acquisition engagement strategies.

### Analysis : New Customer vs Returning Customer
![new vs returning customer](https://github.com/user-attachments/assets/0bbca28d-125a-49e2-87a5-9f7bb03e8447)
#### 1. Overview
The data shows the monthly distribution of new customers and returning customers from January to November. Returning customers consistently form the majority, but the proportion of new customers increases over time. Below are detailed observations:
##### 2. Key Observations
1.	New Customers:
-	Steady Growth:
New customer numbers increased consistently from 740 in January to 1680 in November.
-	Largest month-over-month growth: November (+340 customers).
-	Percentage increase from January to November: 127%.
2.	Returning Customers:
-	Steady Dominance:
Returning customers make up the larger share, ranging between 2700 in February to 6200 in November.
-	Largest month-over-month growth: October to November (+940 customers).
-	Percentage increase from January to November: 125%.
3.	New vs Returning Customer Share:
-	January: New customers constituted 21.1%, while returning customers formed 78.9%.
-	November: New customers increased to 21.3%, while returning customers held 78.7%, maintaining a similar ratio.
#### 3. Trends and Patterns
1.	Growth Correlation:
-	Both new and returning customer segments grew steadily across the months, indicating that efforts to acquire new customers and retain existing ones were successful.
2.	Returning Customer Dependence:
-	Despite the increase in new customers, the business remains heavily dependent on returning customers, contributing consistently over 75% of total customers each month.
3.	Accelerated Growth in Q4:
-	Both new and returning customers show sharper growth in October and November, possibly driven by seasonal promotions or holiday campaigns.
#### 4. Recommendations
1.	Leverage Returning Customers:
-	Enhance loyalty programs to further boost retention and reward frequent customers, as they form the bulk of the customer base.
2.	Focus on New Customer Growth:
-	While the number of new customers is growing, its proportional share remains stagnant (~21%). Strengthen acquisition campaigns (e.g., referral programs, first-time purchase discounts) to increase this percentage.
3.	Boost Engagement in Q4:
-	Continue leveraging the momentum from October–November campaigns to maximize customer growth during peak seasons.

## BRAND & CATEGORY ANALYSIS
![most profitable brand_Page_3](https://github.com/user-attachments/assets/1b52b19f-dd44-4615-ba9d-188d710dd49f)
### Key Observations:
#### 1. Jeans Category
-	Jeans consistently perform well across all months, with steady growth throughout the year.
-	Highest Revenue: The Jeans category peaks at 41K in November, indicating strong demand, likely driven by holiday sales or end-of-year promotions.
-	Steady Growth: There is a clear increasing trend in revenue from January (17.8K) to November (41K), showing an upward trajectory of nearly 2-3K increase each month from June to November.
#### 2. Outerwear & Coats
-	Peak in November: Outerwear & Coats see their highest revenue of 44.3K in November, showing a seasonal demand spike in colder months.
-	Consistent Growth: Similar to Jeans, Outerwear & Coats experience significant growth, particularly in the second half of the year, starting from September to November.
-	Notable Peak in October: A notable surge occurs in October with a 29.7K revenue, likely due to fall season transitions and preparation for winter.
#### 3. Sweaters
-	Steady Seasonal Demand: Sweaters see growth in the second half of the year, peaking in November (24.7K).
-	Lowest Revenues in Early Months: Revenue in the warmer months (January–April) is lower compared to the later months, with the lowest figures observed in January (10.7K) and February (9.3K).
#### 4. Overall Observations
-	Seasonal Patterns: Both Outerwear & Coats and Sweaters follow clear seasonal patterns, with their highest revenues concentrated in the fall and winter months (September to November).
-	Jeans have consistent demand throughout the year, leading the revenue among the three categories.
-	Revenue Peaks: The months of September, October, and November are the most profitable across all categories, especially for Jeans and Outerwear & Coats.
### Business Implications:
#### 1.	Maximize Promotions During Peak Months:
-	Target promotional campaigns for Jeans, Outerwear & Coats, and Sweaters during the fall (September–November) when revenues surge. Use November's holiday shopping to drive significant sales.
#### 2.	Boost Sweater Sales in Early Months:
-	Sweaters show lower performance in the early months of the year. Introducing early discounts or promoting versatility during cooler months can drive higher sales during slower months (e.g., January–April).
#### 3.	Inventory Management:
-	Ensure sufficient stock of Jeans year-round, as it remains the top revenue-generating category. Focus more on stocking Outerwear & Coats and Sweaters during the second half of the year to meet seasonal demand.
#### 4.	Optimize for Seasonal Demand:
-	Plan marketing campaigns and inventory according to the expected seasonal demand for Outerwear & Coats and Sweaters, particularly in fall and winter months.
#### 5.	Q4 Preparations:
-	Given that October and November consistently show high demand, prepare for increased sales during the end-of-year period. Consider promotions during October and November as the key months for revenue generation.
### Summary:
-	Jeans maintain consistent high demand year-round, leading revenue.
-	Outerwear & Coats and Sweaters peak during the fall/winter season, with a sharp increase in September-November.
-	Optimizing inventory, sales promotions, and marketing strategies according to these trends will be key to maximizing sales and profitability across the year.
## Most Profitable Brands
![most profitable brand_Page_1](https://github.com/user-attachments/assets/52ee0d16-bcab-4558-913c-4ae859dc6761)
### 1. Most Profitable Brands
-	Diesel is the most profitable brand, with total revenue nearing 50K. The dominant product category is Jeans (27K).
-	Calvin Klein ranks second, with revenue around 45K. Its largest category is Jeans (9K), along with significant contributions from other categories.
-	7 For All Mankind excels in the Jeans category, generating 35K, making it highly focused on a single category.
### 2. Brands with Product Diversification
-	Calvin Klein shows a high level of diversification, with major contributions from Outerwear & Coats (9K) and Maternity (9K).
### 3. Dominant Categories
-	Jeans dominate across most brands, including Diesel, Joe’s Jeans, True Religion, and 7 For All Mankind.
-	Outerwear & Coats is led by Carhartt, followed by Columbia and Calvin Klein.
-	The Active category also makes a significant contribution, particularly for Columbia and Hurley.
### 4. Specialist Brands
-	Joe’s Jeans focuses solely on Jeans, with revenue of 21K and no significant contributions from other categories.
-	7 For All Mankind is also heavily reliant on Jeans, with minimal diversification.
### Conclusion
-	Brands focusing on the Jeans category (e.g., Diesel and 7 For All Mankind) tend to achieve the highest revenues.
-	Product diversification, as seen with Calvin Klein, can provide a competitive advantage by spreading revenue risks across multiple categories.
-	Smaller brands may consider diversifying their product range or strengthening their dominant categories to optimize their business strategies.

## RFM Customer Segmentation
![rfm](https://github.com/user-attachments/assets/c8872cef-a443-40df-ad65-b674cfb43f6f)
### 1. Customer Segmentation Overview:
-	Recent Customers (40.46%):
Customers who have made purchases recently. This segment offers significant opportunities for engagement through follow-up marketing campaigns to build loyalty.
-	Can't Lose Them (49.16%):
The most valuable customers with high purchasing behavior (frequent and high-value purchases). They need to be retained through top-notch service and exclusive offers.
-	About to Sleep (1.43%):
Customers who are on the verge of inactivity. They need strong incentives to re-engage, such as big discounts or special programs.
-	Champions (0.03%):
Top-performing customers who are highly loyal and active buyers. This segment is small but critical for long-term loyalty.

### 2. Data Interpretation:
*	Segment Dominance:
The two largest segments are "Recent Customers" and "Can't Lose Them." This indicates the business should focus on these two groups:
-	Ensuring Recent Customers remain engaged.
-	Retaining customers in the "Can't Lose Them" category with strong loyalty strategies.
*	Rescue Opportunities:
-	The "About to Sleep" segment (1.43%) needs attention as they are at risk of inactivity.
-	Win-back campaigns such as re-engagement emails, special discounts, or loyalty incentives could be effective.
*	Attention to Champions:
-	The number of customers in the Champions segment is very small (only 0.03%). This could indicate the need to investigate what causes customers to shift away from this category.

### 3. Strategic Recommendations:
1.	For Recent Customers:
-	Send thank-you emails, product recommendations, and offers to encourage second purchases.
-	Provide a seamless purchasing experience to leave a positive impression.
2.	For Can't Lose Them:
-	Launch a loyalty program emphasizing rewards for their purchases.
-	Offer exclusive deals and encourage them to try new products.
3.	For About to Sleep:
-	Analyze their previous purchase patterns to create personalized offers.
-	Engage them with emotional campaigns, such as birthday greetings with vouchers or targeted ads.
4.	For Champions:
-	Review their data to identify behaviors that can be replicated with other customers.
-	Use them as brand advocates by involving them in referral programs or customer testimonials.

### Conclusion:
This RFM chart shows a balanced distribution with a strong focus on new and loyal customers. However, attention should also be directed toward the "About to Sleep" segment and efforts to increase the number of Champions.

## Summary of Recommendations
### 1.	Increase Average Order Value (AOV):
-	Introduce targeted promotions and product bundling strategies to boost AOV, especially for new users who tend to make lower-value purchases.
### 2.	Sustain Engagement for High-Performing Cohorts:
-	Analyze successful cohorts (e.g., July–October) to replicate effective onboarding and targeted campaigns.
-	Implement retention-focused initiatives, such as loyalty programs, reactivation emails, and exclusive offers, to maintain engagement beyond the third month.
### 3.	Focus on Long-Term Retention:
-	Enhance re-engagement efforts for earlier cohorts (e.g., January–June) to improve retention rates stabilizing at 4–5%.
### 4.	Address Retention Drop-Off in Later Cohorts:
-	For September and October cohorts, develop personalized engagement strategies to reduce drop-off during the second and third months.
### 5.	Optimize Seasonal Sales Strategies:
-	Maximize promotions for Jeans, Outerwear & Coats, and Sweaters during peak fall months (September–November).
-	Promote Sweaters in early months (January–April) with discounts and campaigns to boost sales during slow periods.
### 6.	Strengthen Marketing and Acquisition Campaigns:
-	Replicate successful acquisition strategies seen in later cohorts (e.g., October with 14.58% Month 1 retention) while sustaining post-acquisition engagement.
-	Increase the proportion of new customers through referral programs and first-time purchase discounts.
### 7.	Enhance Customer Segmentation:
-	Champions: Use loyalty programs and brand advocacy opportunities (e.g., referral programs).
-	At-Risk Groups: Personalize re-engagement campaigns, including emotional messaging and tailored offers to bring them back.
-	New Customers: Encourage repeat purchases through seamless shopping experiences and thank-you offers.
### 8.	Inventory and Demand Planning:
-	Maintain sufficient inventory for Jeans, as it is the top revenue generator year-round.
-	Stock up on Outerwear & Coats and Sweaters in the second half of the year to meet fall and winter demand.

## Overall Conclusion
To drive growth and optimize profitability, businesses should adopt a dual approach that balances acquisition and retention strategies while aligning product promotions and inventory with seasonal demand. Leveraging insights from successful cohorts and targeting new customers with tailored offers will ensure sustained engagement and revenue. Additionally, effective inventory management and seasonal marketing campaigns will capitalize on high-demand periods, such as Q4, to maximize sales and long-term customer loyalty.


