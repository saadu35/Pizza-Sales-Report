# Sales-Report

## Table of Contents
[Project Overview](#project-overview)

[Data Source](#data-source)

[Tools](#tools)

[Key Performance Indicators (KPIs) Requirements](#kpis-requirements)

[Charts Requirements](#charts-requirements)

[Data Analysis](#data-analysis)

[Results/Findings](#resultsfindings)

[Recommendations](#recommendations)

[Limitations](#limitations)



## Project Overview
---
The objective of this data analysis project is to gain insights into the business performance of a pizza company by analyzing key indicators from the provided sales data. The analysis will focus on calculating metrics such as Total Revenue, Average Order Value, Total Pizzas Sold, Total Orders, and Average Pizzas per Order. Additionally, various charts will be created to visualize trends and patterns in the data.


![Power BI REPORT HOME](https://github.com/saadu35/Pizza-Sales-Report/assets/130612554/94b757a9-c3a6-487d-b3df-1d2ba09490ef)



![POWERBI BW ](https://github.com/saadu35/Pizza-Sales-Report/assets/130612554/9a197486-76ea-4039-b684-2c4dba78c8ec)

---

### Data Source 
**Sales Data:** The primary dataset used for this analysis is the " pizza_sales.csv" file, which contains detailed information about each sale made by the company.

---

### Tools

- **Excel -** Data Cleaning [Download here](https://www.microsoft.com/en-us/microsoft-365/excel)
- **MS SQL Server -** Data Analysis [Download here](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
- **Power BI -** Creating reports [Download here](https://powerbi.microsoft.com/en-us/downloads/)

---

### KPI’s Requirements

I analyze key indicators for our pizza sales data to gain insights into the business performance. Specifically, i want to calculate the following metrics:

**1. Total Revenue:**
- The sum of the total price of all pizza orders.
- Formula: Total Revenue = Σ Total Price of all Orders
  
**2. Average Order Value:**
- The average amount spent per order, calculated by dividing the total revenue by the total number of orders.
- Formula: Average Order Value = Total Revenue / Total Number of Orders

**3. Total Pizzas Sold:**
- The sum of the quantities of all pizzas sold.
- Formula: Total Pizzas Sold = Σ Quantity of Pizzas Sold

**4. Total Orders:**
- The total number of orders placed
- Formula: Total Orders = Count of Unique Order IDs

**5. Average Pizzas per Order:**
- The average number of pizzas sold per order is calculated by dividing the total number of pizzas sold by the total number of orders.
- Formula: Average Pizzas per Order = Total Pizzas Sold / Total Orders

---
### Charts Requirements

i would like to visualize various aspects of our pizza sales data to gain insights and understand key trends. i have identified the following requirements for creating charts:

**1. Daily Trend for Total Orders:**
Create a bar chart that displays the daily trend of orders over a specific time period. This chart will help us identify any patterns or fluctuations in order volumes on a daily basis.

**2. Monthly Trend for Total Orders:**
Create a line chart that illustrates the hourly trend of total orders throughout the day. This chart will allow us to identify peak hours or periods of high-order activity. 

**3. Percentage of Sales by Pizza Category:**
Create a pie chart that shows the distribution of sales across different pizza categories. This chart will provide insights into the popularity of various pizza categories and tier contribution to overall sales. 

**4. Percentages of Sales by Pizza Size:**
Generate a pie chart that represents the percentage of sales attributed to different pizza sizes. This chart will help us understand customer preferences for pizza sizes and their impact on sales.

**5. Total Pizzas Sold by Pizza Category:**
Create a funnel chart that presents the total number of pizzas sold for each pizza category. This chart will allow us to compare the sales performance of different pizza categories.

**6. Top 5 Best Sellers by Revenue, Total Quantity, and Total Orders:**
Create a bar chart highlighting the top 5 best-selling pizzas based on revenue, total quantity, and total orders. This chart will help us identify the most popular pizza options. 

**7. Bottom 5 Best Sellers by Revenue, Total Quantity, and Total Orders:**
Create a bar chart showcasing the bottom 5 worst-selling pizzas based on revenue, total quantity, and total orders. This chart will enable us to identify underperforming or less popular pizza options. 

---

### Data Analysis
**1. Data Cleaning (Power Query/Excel):**
- Remove duplicate records.
- Handle missing values.
- Check for outliers.

**2. Data Analysis (MS SQL Server):**
- Calculate KPIs: Total Revenue, Average Order Value, Total Pizzas Sold, Total Orders, Average Pizzas per Order.

**3. Visualization (Power BI):**
- Create the specified charts based on the defined requirements.

  ---
  
#### Pizza Sales SQL Queries
 ```SQL
 Total Revenue
 SELECT SUM(total_price) AS Total_Revenue FROM pizza_sales;

Average Order Value
SELECT SUM(total_price) / COUNT(DISTINCT order_id) AS Avg_order_Value FROM pizza_sales;

Total Pizzas Sold
SELECT SUM(quantity) AS Total_Pizza_Sold FROM pizza_sales;

Total Orders
SELECT COUNT(DISTINCT order_id) AS Total_Orders FROM pizza_sales

Average Pizzas per Order
SELECT CAST(CAST(SUM(quantity) AS DECIMAL (10,2))/ 
CAST(COUNT(DISTINCT order_id) AS DECIMAL (10,2)) AS DECIMAL (10,2)) AS Avg_pizza_per_order FROM pizza_sales;
```

#### Charts Requirements

```SQL
Daily Trend for Total Orders
SELECT DATENAME(DW,order_date) AS order_day, COUNT(DISTINCT order_id) AS Total_orders from pizza_sales
GROUP BY DATENAME(DW, order_date);

Monthly Trend for Total Orders
SELECT DATENAME(MONTH, order_date) AS Month_Name, COUNT(DISTINCT order_id) AS Total_Order FROM pizza_sales 
GROUP BY DATENAME(MONTH, order_date);

Percentage of Sales by Pizza Category
SELECT pizza_category, SUM(total_price) AS Total_Sales, SUM(total_price) * 100 / (SELECT SUM(total_price) FROM pizza_sales WHERE MONTH(order_date) = 1) 
AS PCT
FROM pizza_sales 
WHERE MONTH(order_date) = 1
GROUP BY pizza_category

Percentages of Sales by Pizza Size
SELECT pizza_size, CAST(SUM(total_price) AS DECIMAL (10,2)) AS Total_Sales, CAST(SUM(total_price) * 100 /
(SELECT SUM(total_price) FROM pizza_sales WHERE DATEPART(quarter, order_date) = 1) AS DECIMAL (10,2))
AS PCT
FROM pizza_sales 
WHERE DATEPART(quarter, order_date) = 1 
GROUP BY pizza_Size
ORDER BY PCT DESC

Total Pizzas Sold by Pizza Category
SELECT pizza_category, SUM(quantity) AS Total_Quantity_sold FROM pizza_sales 
WHERE MONTH(order_date) = 2 
GROUP BY pizza_category
ORDER BY Total_Quantity_sold DESC

Top 5 Best Sellers by Revenue, Total Quantity, and Total Orders
SELECT TOP 5 pizza_name, SUM(quantity) AS Total_quantity FROM pizza_sales
GROUP BY pizza_name 
ORDER BY Total_quantity DESC

Bottom 5 Best Sellers by Revenue, Total Quantity, and Total Orders
SELECT TOP 5 pizza_name, SUM(total_price) AS Total_Revenue FROM pizza_sales
GROUP BY pizza_name 
ORDER BY Total_Revenue ASC

```

---

### Results/Findings
**1. Total Revenue:**
- Identify the overall financial performance of the business.

**2. Average Order Value:**
- Understand customer spending habits.

**3. Total Pizzas Sold:**
- Assess the overall demand for pizzas.

**4. Total Orders:**
- Analyze the volume of orders placed.

**5. Average Pizzas per Order:**
- Understand the average order size.

**6. Charts:**
- Visualize daily and monthly trends, sales distribution by category and size, and performance of top/bottom sellers.

---

### Recommendations
**1. Top Sellers:**
- Focus marketing efforts on popular pizzas to maximize revenue.

**2. Underperforming Pizzas:**
- Evaluate and consider adjusting pricing or marketing strategies for less popular pizzas.

**3. Customer Preferences:**
- Use insights from size distribution to optimize inventory and pricing strategies.

**4. Peak Hours:**
- Staff appropriately during peak hours to handle increased order volumes.

---

### Limitations 
I removed duplicate records to enhance data integrity. However, it's important to acknowledge that the decision to eliminate duplicates introduces a limitation, as the exact impact of these duplicates on the accuracy of calculated metrics and insights remains uncertain. I also handled missing values appropriately, recognizing that the chosen imputation or removal strategies could introduce limitations in influencing the final analysis results. The identification and handling of outliers were crucial steps I took to maintain data quality; nevertheless, the specific criteria for outlier detection and the chosen treatment methods might introduce limitations in how metrics and visualizations are influenced. 

### References
[Youtube](https://www.youtube.com)

[Stackoverflow](https://stackoverflow.com/)
