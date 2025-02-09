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

Deliverable:

1.Queries showcasing window functions, subqueries, and CTEs

2.A report with observations

Outputs for Task2

Window Function Query Ranks employees by their salaries in descending order.
Q21

Subquery Lists employees earning above the average salary.
Q22

CTE (Common Table Expression) Filters employees earning more than $50,000 and displays their details.
Q23

Explanation:

Window Function:

RANK() provides a ranking for employees based on their salary.

Useful for comparisons or hierarchical analysis.

Subquery:

Calculates the average salary dynamically and uses it to filter employees.

Ideal for relative comparisons.
/* USE WINDOW FUNCTIONS,SUBQUERIES, AND CTES (COMMON TABLE EXPRESSIONS) FOR ADVANCED DATA ANALYSIS */

/* 1. Window Function Query */
SELECT name, salary, RANK() OVER (ORDER BY salary DESC) AS rankwise
FROM employees;


/* 2. Subquery */
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

/* 3. CTE (Common Table Expression) */
SELECT name, salary
FROM employees
WHERE salary > 50000;

CTE(COMMON TABLE EXPRESSIONS):

Creates a reusable subset of data for employees with high salaries.

Simplifies complex queries and improves readability.
