# Zomato_Restaurant_Analysis

## Project Overview

**Project Title**: Zomato_Restaurant_Analysis

This project is designed to demonstrate SQL skills and data analysis techniques applied to the Zomato restaurant dataset. The work involved exploring datasets and performing exploratory data analysis (EDA) to uncover patterns and insights about the restaurant industry. I wrote SQL queries to answer key business questions, including overall market statistics, city-wise performance, pricing segments, service offerings, and cuisine popularity. The analysis also compares budget vs premium restaurants, online vs dine-in services, and provides global insights such as dining costs across countries and service feature adoption.

## Objectives

1. **Analyze the Indian restaurant market and uncover overall industry statistics.
2. **Identify top-performing cities based on restaurant count, customer ratings, and dining costs.
3. **Explore the relationship between pricing segments, service offerings (online vs dine-in), and customer satisfaction.
4. **Determine the most popular cuisines and compare performance across budget vs premium restaurant segments.

```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
select *
from R_Sales
where sale_date='2022-11-05'
;
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
select *
from R_Sales
           where category = 'clothing'
           and quantiy>=4
           and sale_date='2022-11-05'
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
select
      category, SUM(total_sale) as 'Total_sale',
      COUNT(*) as 'Total_orders'
from R_sales
group by category
order by total_sale desc
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
select
      category, avg(age) as'Avg_age',
      COUNT(*) as 'No_of_customers'
from R_sales
where category='Beauty'
group by category
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
select *
from R_sales
where total_sale>1000
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
select
     category,
     gender,
     COUNT(transactions_id) as 'Total_Transactions'
from R_sales
group
     by
     category,
     gender
order
     by
     Total_transactions desc
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
WITH MonthlySales AS (
    SELECT
        YEAR(sale_date) AS sales_year,
        MONTH(sale_date) AS sales_month,
        AVG(total_sale) AS avg_monthly_sale
    FROM R_sales
    GROUP BY YEAR(sale_date), MONTH(sale_date)
),
RankedSales AS (
    SELECT
        sales_year,
        sales_month,
        avg_monthly_sale,
        RANK() OVER (PARTITION BY sales_year ORDER BY avg_monthly_sale DESC) AS sales_rank
    FROM MonthlySales
)
SELECT
    sales_year,
    sales_month,
    avg_monthly_sale,
    sales_rank
FROM RankedSales
WHERE sales_rank = 1;   -- Best month per year
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
SELECT TOP 5
    customer_id,
    SUM(total_sale_amount) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC;
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
```sql
SELECT
      category,
      COUNT(DISTINCT customer_id) AS unique_customers
FROM R_sales
GROUP BY
      category;
```

## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project provided me with hands-on experience in applying SQL to real-world business problems. From creating and managing databases to performing data cleaning, exploratory analysis, and advanced business-driven queries, I was able to demonstrate my ability to transform raw sales data into meaningful insights. The analysis highlighted sales patterns, customer behavior, and product performance, which are critical for strategic decision-making. By completing this project, I not only strengthened my SQL skills but also showcased my capability to think analytically and solve problems that directly impact business outcomes—skills that are highly valuable for a Business Analyst or Data Analyst role.
