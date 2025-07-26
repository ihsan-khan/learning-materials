# ⭐️📚Top 25 SQL questions to crack any SQL interviews

## 1 Write a query to find the second highest salary in an employee table.

<details>
	<summary><b>View Query</b></summary>
<ul>


Here are several ways to find the second highest salary from an employees table:

## Method 1: Using LIMIT and OFFSET (MySQL, PostgreSQL, SQLite)
```sql
SELECT salary FROM employees ORDER BY salary DESC LIMIT 1 OFFSET 1;
```

## Method 2: Using Subquery (Works in most databases)

```sql
SELECT MAX(salary) AS second_highest_salary
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
```
</ul>
</details>

## 2 Fetch all employees whose names contain the letter “a” exactly twice.

```sql
SELECT * FROM employee WHERE LENGTH(name) - LENGTH(REPLACE(name, 'a', '')) = 2;
```

## 3 How do you retrieve only duplicate records FROM a table?

```sql
SELECT email, COUNT(*) FROM employee GROUP BY email HAVING COUNT(*) > 1;
```

```sql
SELECT *
FROM employee e1
WHERE EXISTS (
    SELECT 1
    FROM employee e2
    WHERE e1.email = e2.email
      AND e1.id <> e2.id
);
```

## 4 Write a query to calculate the running total of sales by date.

```sql
SELECT 
  s1.sales_date,
  s1.sales_amount,
  (
    SELECT SUM(sales_amount) FROM 
    sales as s2 WHERE s2.sales_date <= s1.sales_date 
  ) AS running_total
sales as s1
ORDER BY s1.sales_date
```

## 5 Find employees who earn more than the average salary in their department.

```sql
SELECT * FROM
employee e WHERE salary > (
    SELECT AVG(salary) FROM employee WHERE e.department_id = employee.department_id
)
```

## 6 Write a query to find the most frequently occurring value in a column.

```sql
SELECT column_name, COUNT(*) as frequency from table GROUP BY column_name ORDER BY frequency DESC LIMIT 1;
```

## 7 Fetch records where the date is within the last 7 days from today.

```sql
SELECT * FROM table_name WHERE date_column >= CURDATE() - INTERVAL 7 DAY;
SELECT * FROM table_name WHERE date_column >= DATE_SUB(CURDATE(), INTERVAL 7 DAY);
```

## 8 Write a query to count how many employees share the same salary.

```sql
SELECT  salary, COUNT(*) AS employee_count FROM employee GROUP BY salary;
```

## 9 How do you fetch the top 3 records for each group in a table?

```sql
SELECT e1.* FROM employee e1
WHERE (
    SELECT COUNT(*) 
    FROM employee e2
    WHERE e1.department_id = e2.department_id AND
    salary e2.salary > e1.salary 
) < 3 
ORDER BY department_id, salary DESC;
```

## 10 Retrieve products that were never sold (hint: use LEFT JOIN).

```sql
SELECT p.*
FROM products p
LEFT JOIN sales s ON p.product_id = s.product_id
WHERE s.product_id IS NULL;
```

# 💡 Challenging Level:
## 1 Retrieve customers who made their first purchase in the last 6 months.

```sql
SELECT customer_id, MIN(purchase_date) AS first_purchase_date
FROM sales
GROUP BY customer_id
HAVING MIN(purchase_date) >= DATE_SUB(CURDATE(), INTERVAL 6 MONTH);
```

## 2 How do you pivot a table to convert rows into columns?

```sql
SELECT
product_id,
    SUM(CASE WHEN year = 2021 THEN sales_amount ELSE 0 END) AS sales_2021,
    SUM(CASE WHEN year = 2022 THEN sales_amount ELSE 0 END) AS sales_2022
FROM sales
GROUP BY product_id;
```

## 3 Write a query to calculate the percentage change in sales month-over-month.

```sql
SELECT
current_month.sales_month,
    current_month.total_sales,
    previous_month.total_sales AS previous_month_sales,
    CASE 
        WHEN previous_month.total_sales IS NULL THEN 0
        ELSE ROUND((current_month.total_sales - previous_month.total_sales) 
                   / previous_month.total_sales * 100, 2)
    END AS percentage_change
FROM 
    (SELECT DATE_FORMAT(sales_date, '%Y-%m') AS sales_month, SUM(sales_amount) AS total_sales
     FROM sales
     GROUP BY DATE_FORMAT(sales_date, '%Y-%m')) AS current_month
LEFT JOIN 
    (SELECT DATE_FORMAT(sales_date, '%Y-%m') AS sales_month, SUM(sales_amount) AS total_sales
     FROM sales
     GROUP BY DATE_FORMAT(sales_date, '%Y-%m')) AS previous_month
ON DATE_SUB(current_month.sales_month, INTERVAL 1 MONTH) = previous_month.sales_month
ORDER BY current_month.sales_month;
```

## 4 Find the median salary of employees in a table.

```sql
SELECT AVG(salary) AS median_salary
FROM (
    SELECT salary
    FROM employee
    ORDER BY salary
    LIMIT 1 OFFSET (SELECT FLOOR((COUNT(*) - 1) / 2) FROM employee)
) AS sub1
UNION ALL
SELECT AVG(salary)
FROM (
    SELECT salary
    FROM employee
    ORDER BY salary
    LIMIT 2 OFFSET (SELECT FLOOR((COUNT(*) - 1) / 2) FROM employee)
) AS sub2
WHERE (SELECT COUNT(*) FROM employee) % 2 = 0;
```

## 5 Fetch all users who logged in consecutively for 3 days or more.

```sql
SELECT DISTINCT l1.user_id
FROM logins l1
JOIN logins l2 ON l1.user_id = l2.user_id 
               AND l2.login_date = DATE_ADD(l1.login_date, INTERVAL 1 DAY)
JOIN logins l3 ON l1.user_id = l3.user_id 
               AND l3.login_date = DATE_ADD(l2.login_date, INTERVAL 1 DAY);
```

## 6 Write a query to delete duplicate rows while keeping one occurrence.

```sql
DELETE u1
FROM users u1
JOIN users u2 
ON u1.first_name = u2.first_name
   AND u1.last_name = u2.last_name
   AND u1.email = u2.email
   AND u1.id > u2.id;
```

## 7 Create a query to calculate the ratio of sales between two categories.

```sql
SELECT
SUM(CASE WHEN category = 'Electronics' THEN amount ELSE 0 END) AS electronics_sales,
    SUM(CASE WHEN category = 'Furniture' THEN amount ELSE 0 END) AS furniture_sales,
    SUM(CASE WHEN category = 'Electronics' THEN amount ELSE 0 END) /
    SUM(CASE WHEN category = 'Furniture' THEN amount ELSE NULL END) AS sales_ratio
FROM sales;
```

## 8 How would you implement a recursive query to generate a hierarchical structure? 

```sql
WITH RECURSIVE employee_hierarchy AS (
SELECT 
        id,
        name,
        manager_id,
        1 AS level
    FROM employees
    WHERE manager_id IS NULL
UNION ALL
SELECT
e.id,
        e.name,
        e.manager_id,
        eh.level + 1 AS level
    FROM employees e
    INNER JOIN employee_hierarchy eh
    ON e.manager_id = eh.id
)
SELECT 
    CONCAT(REPEAT('    ', level - 1), name) AS hierarchy,
    level
FROM employee_hierarchy
ORDER BY level, manager_id;
```

## 9 Write a query to find gaps in sequential numbering within a table.

```sql
SELECT
t1.id + 1 AS start_of_gap,
    t2.id - 1 AS end_of_gap
FROM numbers t1
LEFT JOIN numbers t2 
    ON t1.id + 1 = t2.id
WHERE t2.id IS NULL;
```

## 10 Split a comma-separated string into individual rows using SQL. 

```sql
SELECT id,
SUBSTRING(product_names, FIND_IN_SET(',', product_names, n) + 1, 
                 FIND_IN_SET(',', product_names, n+1) - FIND_IN_SET(',', product_names, n) - 1) AS product_name
FROM products
JOIN (SELECT @n := 0) vars,
     (SELECT @n := @n + 1 AS n FROM products LIMIT 10) numbers; -- Adjust 10 as necessary.
```

# 💡 Advanced Problem-Solving:

## 1 Rank products by sales in descending order for each region.

```sql
SELECT region, 
       product_id, 
       product_name, 
       quantity_sold,
       (
           SELECT COUNT(*) 
           FROM sales s2 
           WHERE s2.region = s1.region 
           AND s2.quantity_sold > s1.quantity_sold
       ) + 1 AS rank
FROM sales s1
JOIN products p ON s1.product_id = p.product_id
ORDER BY region, rank;
```

## 2 Fetch all employees whose salaries fall within the top 10% of their department.

```sql
WITH department_counts AS (
SELECT department_id, COUNT(*) AS total_count
    FROM employees
    GROUP BY department_id
),
top_10_percent_threshold AS (
    SELECT department_id, 
           (total_count * 0.1) AS top_10_count
    FROM department_counts
)
SELECT e.*
FROM employees e
JOIN top_10_percent_threshold tpt ON e.department_id = tpt.department_id
WHERE e.salary >= (
    SELECT MAX(salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
    LIMIT tpt.top_10_count, 1
);
```

## 3 Identify orders placed during business hours (e.g., 9 AM to 6 PM).

```sql
SELECT order_id, order_time, amount
FROM orders
WHERE HOUR(order_time) BETWEEN 9 AND 18;
```

## 4 Write a query to get the count of users active on both weekdays and weekends.

```sql
SELECT user_id
FROM (
    SELECT user_id
    FROM user_activity
    WHERE WEEKDAY(login_time) IN (0, 1, 2, 3, 4) -- Weekdays
) AS weekday_activity
JOIN (
    SELECT user_id
    FROM user_activity
    WHERE WEEKDAY(login_time) IN (5, 6) -- Weekends
) AS weekend_activity ON weekday_activity.user_id = weekend_activity.user_id
GROUP BY user_id;
```

```sql
SELECT user_id
FROM user_activity
WHERE (
    WEEKDAY(login_time) IN (0, 1, 2, 3, 4) -- Weekdays
    AND WEEKDAY(login_time) IN (5, 6) -- Weekends
)
GROUP BY user_id;
```

## 5 Retrieve customers who made purchases across at least three different categories.

```sql
SELECT customer_id
FROM purchases
GROUP BY customer_id
HAVING COUNT(DISTINCT category_id) >= 3;
```
