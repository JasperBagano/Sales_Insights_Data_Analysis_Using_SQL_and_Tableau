# Sales_Insights_Data_Analysis_Using_SQL_and_Tableau

I am sharing a personal project performing data analysis of an eletronics company's sales data using SQL and Tableau.

### Objective:
The goal is to analyze the customer behavior and sales performance of electronic products to derive actionable insights on customer demographics, product preferences, payment methods, and order status. The tools I used were **MySQL** for data storage and **Tableau** for analysis and visualization.

### Problem Statement:
The electronics company is facing challenges in understanding customer behavior, product performance, and order fulfillment processes. Despite having extensive sales data, the company struggles to derive actionable insights that could help improve decision-making and optimize business operations.
1. Time-Based Sales Trends:
   - What are the key trends in monthly sales, and how do seasonal variations impact revenue?
   - Are there any patterns that can be leveraged to optimize marketing and inventory strategies?
2. Product Performance:
    - Which product types and SKUs are driving the most revenue, and what factors (such as customer ratings) are influencing sales performance?
    - Which product types and SKUs are cancelled most of the time which result to lost sales?
3. Customer Demographics:
    - How does customer age, gender, and loyalty status influence purchasing decisions?
    - What is the profile of high-value customers, and how can loyalty programs be optimized to increase retention?
4. Order Fulfillment and Payment Methods:
    - What is the rate of order cancellations, and how do factors like payment methods and shipping options affect order completion?
    - Which payment methods are most popular, and how do they impact the overall sales process?

### Implementation Plan:
1. Purpose:
   - To reveal hidden sales patterns and customer insights that can enhance strategic decision-making for the sales and marketing teams. The goal is to automate sales reporting to minimize manual data processing, leading to better resource utilization and improved operational efficiency.
2. Stakeholders:
   - Director of Sales
   - Marketing Team
   - Finance Team
   - IT and Data Infrastructure Team
   - Data & Analytics Team
3. Output
   - A dynamic and automated Tableau dashboard that provides up-to-date sales insights and customer analytics to drive data-driven decisions. This will empower teams to better understand customer preferences, identify product performance trends, and streamline the decision-making process for inventory, marketing, and promotions.
4. Success Factors
   - Dashboards offering comprehensive insights into customer demographics, product performance, and order fulfillment, with the latest data readily accessible.
   - Sales and marketing teams making informed decisions that result in a 15% increase in customer satisfaction and product alignment with demand.
   - A 25% reduction in manual data extraction efforts, allowing sales analysts to reinvest their time in strategic activities and contribute to a 10% revenue boost through targeted campaigns and improved inventory management.

### Data Analysis Using SQL
***
1. Show order count by gender, split by order status.
~~~
SELECT Gender,
   SUM(CASE WHEN Order_Status = 'Completed' THEN 1 ELSE 0 END) AS Completed_Orders,
   SUM(CASE WHEN Order_Status = 'Cancelled' THEN 1 ELSE 0 END) AS Cancelled_Orders,
   COUNT(*) AS Total_Orders
FROM projects.electronic_sales
GROUP BY Gender;
~~~
2. Show order count by payment method, split by order status.
~~~
SELECT Payment_Method, 
   SUM(CASE WHEN Order_Status = 'Completed' THEN 1 ELSE 0 END) AS Completed_Orders,
   SUM(CASE WHEN Order_Status = 'Cancelled' THEN 1 ELSE 0 END) AS Cancelled_Orders,
   COUNT(*) AS Total_Orders
FROM projects.electronic_sales
GROUP BY Payment_Method;
~~~
3. Show order count by shipping type, split by order status.
~~~
SELECT Shipping_Type, 
   SUM(CASE WHEN Order_Status = 'Completed' THEN 1 ELSE 0 END) AS Completed_Orders,
   SUM(CASE WHEN Order_Status = 'Cancelled' THEN 1 ELSE 0 END) AS Cancelled_Orders,
   COUNT(*) AS Total_Orders
FROM projects.electronic_sales
GROUP BY Shipping_Type;
~~~
4. Show order count by customer age.
~~~
SELECT Age,
   SUM(CASE WHEN Order_Status = 'Completed' THEN 1 ELSE 0 END) AS Completed_Orders,
   SUM(CASE WHEN Order_Status = 'Cancelled' THEN 1 ELSE 0 END) AS Cancelled_Orders, count(*) AS Total_Orders
FROM projects.electronic_sales
GROUP BY Age
ORDER BY Age;
~~~
5. Show sales totals split by order status.
~~~
SELECT Order_Status,
   COUNT(*) AS Sales_Count,
   ROUND(SUM(Total_Price),0) AS Sales_Total,
   ROUND(SUM(Add_on_Total),0) AS Add_On_Total,
   ROUND(SUM(Total_Price)+SUM(Add_on_Total),0) AS Gross_Sales
FROM projects.electronic_sales
GROUP BY Order_Status
