# DATA-ANALYSIS-WITH-COMPLEX-QUERIES
Name: Kashish Jiyani

Company: CODTECH IT SOLUTIONS

ID :CT08LQI

Domain: SQL

Tasks Duration: January 10th,2025 to February 10th, 2025.

Mentor:Santhosh

Task 2: Advanced Data Analysis

Objective: Use advanced SQL techniques such as window functions, subqueries, and CTEs(COMMON TABLE EXPRESSIONS) for complex data analysis.

Purpose: To extract trends, rankings, or patterns, which are common in analytics tasks, using advanced SQL capabilities.





To meet the requirement of generating a report showcasing trends or patterns using SQL queries with advanced techniques like **Window Functions**, **Subqueries**, and **CTEs (Common Table Expressions)**, we can structure the report generation as follows:

### **1. Setup: Data and Context**
Assume we are working with a business data set, such as sales, customer interactions, or transactions, with relevant tables. Below are the table structures:

- **Sales**:
  - `sale_id` (INT)
  - `sale_date` (DATE)
  - `amount` (DECIMAL)
  - `customer_id` (INT)
  
- **Customers**:
  - `customer_id` (INT)
  - `customer_name` (VARCHAR)
  - `region` (VARCHAR)
  
- **Products**:
  - `product_id` (INT)
  - `product_name` (VARCHAR)
  - `price` (DECIMAL)

### **2. SQL Query Using Window Functions, Subqueries, and CTEs**

We'll create a complex query to analyze sales data trends, customer behavior, and identify the highest sales regions over the past year. The SQL query includes:

- **Window Functions** for ranking and partitioning sales data.
- **CTEs** for aggregating and organizing data.
- **Subqueries** for calculating sales patterns and analyzing customer engagement.

```sql
WITH 
-- CTE to get total sales by region
Regional_Sales AS (
    SELECT 
        s.sale_date,
        c.region,
        SUM(s.amount) AS total_sales
    FROM Sales s
    JOIN Customers c ON s.customer_id = c.customer_id
    WHERE s.sale_date >= DATE_SUB(CURRENT_DATE, INTERVAL 1 YEAR)  -- Last year’s sales
    GROUP BY s.sale_date, c.region
),

-- CTE to calculate the rank of regions based on total sales
Region_Rank AS (
    SELECT 
        region,
        total_sales,
        RANK() OVER (ORDER BY total_sales DESC) AS sales_rank
    FROM Regional_Sales
    GROUP BY region, total_sales
),

-- CTE to get the most frequent products sold in each region
Frequent_Products AS (
    SELECT 
        s.region,
        p.product_name,
        COUNT(s.sale_id) AS product_sales_count
    FROM Sales s
    JOIN Products p ON s.product_id = p.product_id
    JOIN Customers c ON s.customer_id = c.customer_id
    GROUP BY s.region, p.product_name
    HAVING COUNT(s.sale_id) > 50  -- Consider only popular products (e.g., sold more than 50 times)
),

-- Subquery to find the average sales per region over the last year
Avg_Regional_Sales AS (
    SELECT 
        region,
        AVG(total_sales) AS avg_sales
    FROM Regional_Sales
    GROUP BY region
)

-- Final report combining results
SELECT 
    rs.region,
    rs.total_sales,
    rr.sales_rank,
    fp.product_name AS most_frequent_product,
    fp.product_sales_count,
    ars.avg_sales
FROM Regional_Sales rs
JOIN Region_Rank rr ON rs.region = rr.region
JOIN Frequent_Products fp ON rs.region = fp.region
JOIN Avg_Regional_Sales ars ON rs.region = ars.region
WHERE rr.sales_rank <= 5  -- Only top 5 regions by sales
ORDER BY rr.sales_rank, fp.product_sales_count DESC;
```

### **Explanation of Key Sections**:

1. **CTE `Regional_Sales`**: 
   - This aggregates the total sales by region over the past year. It joins the `Sales` table with the `Customers` table to determine the region of each sale.

2. **CTE `Region_Rank`**: 
   - This ranks regions based on their total sales using the `RANK()` window function. The regions are sorted in descending order of total sales, and the rank is assigned accordingly.

3. **CTE `Frequent_Products`**: 
   - This identifies the most frequent products sold in each region. We use `COUNT(s.sale_id)` to calculate how often each product has been sold and filter for those with a significant number of sales.

4. **CTE `Avg_Regional_Sales`**: 
   - This calculates the average sales per region over the past year. This can be used to identify which regions are performing better than average.

5. **Final Select**:
   - The final report pulls together all the information. It combines the total sales, regional rank, most frequent product, and average regional sales, filtered to show only the top 5 regions by sales. The `ORDER BY` ensures that the results are listed by sales rank and product frequency.

### **Sample Output**:

| Region    | Total Sales | Sales Rank | Most Frequent Product | Product Sales Count | Avg Sales |
|-----------|-------------|------------|-----------------------|---------------------|-----------|
| North     | 1,200,000   | 1          | Laptop                | 100                 | 950,000   |
| West      | 900,000     | 2          | Smartphone            | 80                  | 875,000   |
| South     | 800,000     | 3          | Headphones            | 70                  | 825,000   |
| East      | 750,000     | 4          | Tablet                | 60                  | 800,000   |
| Central   | 700,000     | 5          | Camera                | 55                  | 775,000   |

### **3. Trends and Insights**:

This query allows us to identify:
- The regions with the highest sales over the past year.
- The most frequent products sold in these regions.
- The average sales per region, highlighting high-performing regions.
- Trends in product sales, enabling businesses to adjust inventory or marketing efforts.

### **4. Conclusion**:

By leveraging **Window Functions**, **CTEs**, and **Subqueries**, we’ve built a comprehensive SQL query that generates a report analyzing sales trends, product performance, and regional differences. This approach can be adapted for various business datasets and used for more detailed analysis and decision-making.

This SQL query structure is a great starting point for advanced data analysis in business environments. You can expand on this with more complex joins, filters, and additional analytical functions as needed for your reporting purposes.
