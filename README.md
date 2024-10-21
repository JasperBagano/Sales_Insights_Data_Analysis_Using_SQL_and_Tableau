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
~~~
6. Show monthly sales split by main product and add-on product, and month-on-month growth.
~~~
WITH Sales_CTE AS (
    SELECT 
        YEAR(Purchase_Date) AS Sales_Year, 
        MONTH(Purchase_Date) AS Sales_Month, 
        COUNT(*) AS Sales_Count, 
        ROUND(SUM(Total_Price), 0) AS Sales_Total, 
        ROUND(SUM(Add_on_Total), 0) AS Add_On_Total, 
        ROUND(SUM(Total_Price + Add_on_Total), 0) AS Gross_Sales
    FROM projects.electronic_sales
    WHERE Order_Status = "Completed"
    GROUP BY YEAR(Purchase_Date), MONTH(Purchase_Date)
)
SELECT 
    Sales_Year, 
    Sales_Month, 
    Sales_Count, 
    Sales_Total, 
    Add_On_Total, 
    Gross_Sales, 
    ROUND(
        (Gross_Sales - LAG(Gross_Sales, 1) OVER (ORDER BY Sales_Year, Sales_Month)) / 
        LAG(Gross_Sales, 1) OVER (ORDER BY Sales_Year, Sales_Month) * 100, 2
    ) AS MoM_Growth
FROM Sales_CTE
ORDER BY Sales_Year, Sales_Month;
~~~
7. Show average customer rating for each SKU with corresponding sales order count and quantity.
~~~
SELECT SKU,
   ROUND(AVG(Rating),2) AS Average_Rating,
   SUM(Quantity) AS Total_Quantity,
   COUNT(*) AS Order_Count
FROM projects.electronic_sales
WHERE Order_Status = "Completed"
GROUP BY SKU
ORDER BY AVG(Rating) DESC
~~~
8. Show the SKU that is sold with the most add-on.
~~~
SELECT SKU,
    ROUND(SUM(Add_on_Total),0) AS Total_Add_On
FROM projects.electronic_sales
WHERE Order_Status = "Completed"
GROUP BY SKU
ORDER BY ROUND(SUM(Add_on_Total),0) DESC
~~~
9. Show SKU sold per age groups.
~~~
SELECT SKU,
   COUNT(CASE WHEN Age BETWEEN 10 AND 19 THEN 1 ELSE NULL END) AS "10-19",
   COUNT(CASE WHEN Age BETWEEN 20 AND 29 THEN 1 ELSE NULL END) AS "20-29",
   COUNT(CASE WHEN Age BETWEEN 30 AND 39 THEN 1 ELSE NULL END) AS "30-39",
   COUNT(CASE WHEN Age BETWEEN 40 AND 49 THEN 1 ELSE NULL END) AS "40-49",
   COUNT(CASE WHEN Age BETWEEN 50 AND 59 THEN 1 ELSE NULL END) AS "50-59",
   COUNT(CASE WHEN Age BETWEEN 60 AND 69 THEN 1 ELSE NULL END) AS "60-69",
   COUNT(CASE WHEN Age BETWEEN 70 AND 79 THEN 1 ELSE NULL END) AS "70-79",
   COUNT(CASE WHEN Age BETWEEN 80 AND 89 THEN 1 ELSE NULL END) AS "80-89"
FROM projects.electronic_sales
WHERE Order_Status = "Completed"
GROUP BY SKU;
~~~
10. Show SKU sold by gender.
~~~
SELECT SKU,
   COUNT(CASE WHEN Gender = "Male" THEN 1 ELSE NULL END) AS "Male",
   COUNT(CASE WHEN Gender = "Female" THEN 1 ELSE NULL END) AS "Female"
FROM projects.electronic_sales
WHERE Order_Status = "Completed"
GROUP BY SKU;
~~~

### Data Analysis Using Tableau

Tableau Dashboard - [Sales Analysis](https://public.tableau.com/shared/NNQ65M4NQ?:display_count=n&:origin=viz_share_link).

[![Sales Analysis Dashboard](https://github.com/JasperBagano/Sales_Insights_Data_Analysis_Using_SQL_and_Tableau/blob/e74a506efc734e732508a55d0d41585074310bdc/image_Sales%20Analysis.png)](https://public.tableau.com/views/Electronics_Company_Sales_Insights_Analysis_Using_Tableau/SalesAnalysis?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

Tableau Dashboard - [Customer Behavior](https://public.tableau.com/views/Electronics_Company_Sales_Insights_Analysis_Using_Tableau/CustomerBehavior?:language=en-US&:sid=&:display_count=n&:origin=viz_share_link).

[![Customer Behavior Dashboard](https://github.com/JasperBagano/Sales_Insights_Data_Analysis_Using_SQL_and_Tableau/blob/e74a506efc734e732508a55d0d41585074310bdc/image_Customer%20Behavior.png)](https://public.tableau.com/views/Electronics_Company_Sales_Insights_Analysis_Using_Tableau/CustomerBehavior?:language=en-US&:sid=&:display_count=n&:origin=viz_share_link)

