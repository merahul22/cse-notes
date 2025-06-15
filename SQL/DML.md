

# Data Manipulation Language (DML) – 

**Overview:** DML commands (SELECT, INSERT, UPDATE, DELETE) let analysts _retrieve_ and _modify_ row-level data. DML differs from DDL (schema changes). The primary DML statements are **SELECT** (fetch/query data) and **INSERT/UPDATE/DELETE** (modify data). In practice, analysts mainly write SELECTs to explore and report on data, but knowing how to insert or correct data is also important (often used in data cleaning).

## SELECT (Data Retrieval)

- **Purpose:** Fetch and view data from tables/views. SELECT does _not_ change data, but it can transform and summarize it.
    
- **Basic syntax:**
    
    ```sql
    SELECT column1, column2, …
    FROM table_name
    [WHERE condition]
    [GROUP BY col_list]
    [HAVING aggregate_condition]
    [ORDER BY col_list];
    ```
    
    Clauses execute in order: `SELECT` → `FROM` → `JOIN/ON` → `WHERE` → `GROUP BY` → `HAVING` → `ORDER BY`.
    
- **Selecting columns:** Always list needed columns instead of `SELECT *`. This avoids retrieving unnecessary data and improves performance. For example:
    
    ```sql
    SELECT first_name, last_name, phone
      FROM Employees
      ORDER BY last_name;
    ```
    
    Unlike `SELECT *`, specifying columns reduces data transfer and allows use of covering indexes. Use clear aliases and consistent naming for readability.
    
- **Filtering (WHERE):** Restrict rows with `WHERE`. Supports comparisons (`=, <, >, <>`), ranges (`BETWEEN x AND y`), sets (`IN (… )`), patterns (`LIKE '%text%'`), null checks (`IS NULL / IS NOT NULL`), and boolean logic (`AND, OR`).
    
    - **Example:**
        
        ```sql
        SELECT * 
          FROM Sales
         WHERE qty BETWEEN 20 AND 50;
        ```
        
        This finds sales with `qty >= 20 AND qty <= 50`. Using `IN` is equivalent to multiple ORs:
        
        ```sql
        SELECT * FROM Publishers
         WHERE province IN ('BC','AB','ON');
        ```
        

. Use `%` and `_` wildcards with `LIKE` (e.g., `WHERE name LIKE 'A_%'` finds names starting with ‘A’).

- **Joins (combining tables):** To query multiple tables, use joins on key columns. Common types:
    
    - **INNER JOIN:** Returns only matching rows in both tables. _Unmatched rows are discarded._
        
        ```sql
        SELECT e.last_name, d.name AS department
          FROM Employees e
          JOIN Departments d 
            ON e.dept_id = d.id;
        ```
        
        Here only employees with a matching department appear. (By SQL standard, `JOIN` = inner join.).
        
    - **LEFT (OUTER) JOIN:** Returns _all_ rows from the left table, and matching rows from the right. Unmatched right-side columns are NULL. Use when you want all records from the first table, even if no match.
        
        ```sql
        SELECT c.name, o.order_date
          FROM Customers c
          LEFT JOIN Orders o ON c.id = o.customer_id;
        ```
        
        This lists all customers and their orders; customers without orders show NULL in order fields.
        
    - **RIGHT (OUTER) JOIN:** Opposite of LEFT JOIN: all rows from the right table, NULLs for left. Used less often.
        
    - **FULL OUTER JOIN:** Returns all rows from _both_ tables; non-matching sides are NULL.
        
    - **CROSS JOIN:** Cartesian product (every combination). Rarely used in analytics, but a join without any ON/WHERE conditions.
        
    
    _Pitfall:_ Always include correct ON conditions. Missing or wrong joins can produce huge or empty result sets. Use table aliases to avoid ambiguity.
    
- **Aggregations (GROUP BY):** To compute summaries (counts, sums, averages, etc.), use aggregate functions with `GROUP BY`. `GROUP BY` collapses rows into groups based on the listed columns.
    
    - **Example:** Count sales by year:
        
        ```sql
        SELECT year, COUNT(*) AS sales_count
          FROM sales
         GROUP BY year;
        ```
        
        This returns one row per `year`, with the total count. You can group by multiple columns by separating them with commas.
        
    - **HAVING:** Use `HAVING` to filter groups _after_ aggregation. E.g., find products with total sales ≥ 1000:
        
        ```sql
        SELECT product_id, SUM(amount) AS total_sales
          FROM order_items
         GROUP BY product_id
         HAVING SUM(amount) >= 1000;
        ```
        
        Here `WHERE` cannot be used for the aggregate condition; `HAVING` is needed.
        
- **Subqueries:** A `SELECT` can nest queries. A _non-correlated_ subquery runs once and its result is used by the outer query. A _correlated_ subquery depends on each row of the outer query (runs row-by-row).
    
    - **Example (non-correlated):** Find customers who placed an order in 2023:
        
        ```sql
        SELECT name 
          FROM Customers 
         WHERE id IN (
               SELECT customer_id 
                 FROM Orders 
                WHERE order_date >= '2023-01-01'
               );
        ```
        
        The inner query runs once to list 2023 order customers; the outer query selects those customers.
        
    - **Correlated example:** Find employees earning above their department’s average:
        
        ```sql
        SELECT e.last_name, e.salary
          FROM Employees e
         WHERE e.salary > (
               SELECT AVG(salary)
                 FROM Employees
                WHERE department_id = e.department_id
               );
        ```
        
        The inner query uses `e.department_id` from the outer row; it executes per employee. Correlated subqueries are useful for row-specific comparisons.
        
- **ORDER BY:** Sort results with `ORDER BY column [ASC|DESC]`. E.g., `ORDER BY last_name DESC, first_name`.


## INSERT (Adding Data)

- **Purpose:** Add new row(s) to a table. Common when importing or staging data.
    
- **Syntax (VALUES):**
    
    ```sql
    INSERT INTO table_name (col1, col2, …) 
    VALUES (val1, val2, …);
    ```
    
    List the columns explicitly for safety. Example:
    
    ```sql
    INSERT INTO raw_customers (id, first_name, last_name) 
    VALUES (101, 'Kira', 'F.');  
    ```
    
    This adds a single new customer (the example is adapted from dbt’s guide).
    
- **Syntax (SELECT):** You can also insert multiple rows from another query:
    
    ```sql
    INSERT INTO archive_sales
    SELECT *
      FROM sales
     WHERE sale_date < '2022-01-01';
    ```
    
    This moves old sales into an archive.
    
- **Use-cases:** Populating tables after ETL processes, archiving old data, duplicating test data. Analysts may rarely use INSERT manually – often data pipelines handle it – but knowing the syntax is essential for tasks like creating quick test data or filling a lookup table.
    
- **Best practices:** Always specify columns. Ensure data types and constraints (keys, not-null) are satisfied. Be aware of default values. Many analysts prefer using `INSERT ... SELECT` for bulk operations. Use transactions (`BEGIN`/`COMMIT`) if inserting multiple related rows, so you can roll back on error.
    

## UPDATE (Modifying Data)

- **Purpose:** Change existing row values in a table. Use for data cleaning (fixing typos, standardizing formats) or correcting records. Unlike DDL `ALTER`, `UPDATE` changes row content.
    
- **Syntax:**
    
    ```sql
    UPDATE table_name
       SET col1 = expr1,
           col2 = expr2, …
     [WHERE condition];
    ```
    
    Example:
    
    ```sql
    UPDATE orders
       SET status = 'returned'
     WHERE order_id = 7;
    ```
    
    (Example from dbt: changing an order’s status).
    
- **Batch updates:** You can update multiple rows at once by a broader `WHERE`. E.g. increase all salaries by 5%:
    
    ```sql
    UPDATE employees
       SET salary = salary * 1.05
     WHERE department = 'Engineering';
    ```
    
- **Use-cases:** Correcting data errors (e.g. fix misspelled countries), setting flags/labels, applying business rules. For example, if a country field has typos like “Cnada” or “Canda”, use `UPDATE` to standardize it to “Canada”.
    
- **Important precautions:** Always include a `WHERE` to avoid changing every row. An `UPDATE` without `WHERE` modifies the entire table (often catastrophic). A good practice is to write and test the `WHERE` clause first, or run a `SELECT` with the same conditions to confirm the affected rows. Using `RETURNING` (in databases that support it) can show what was changed. Use transactions so you can rollback if the update has unintended effects.
    

## DELETE (Removing Data)

- **Purpose:** Delete existing rows from a table. Common in cleanup or archiving tasks. (To remove all rows, prefer `TRUNCATE` as it’s faster and doesn’t log row-by-row deletes, but `TRUNCATE` is a DDL operation.)
    
- **Syntax:**
    
    ```sql
    DELETE FROM table_name
     [WHERE condition];
    ```
    
    Example:
    
    ```sql
    DELETE FROM customers
     WHERE signup_date < '2020-01-01';
    ```
    
    This deletes records of customers who signed up before 2020.
    
- **Use-cases:** Remove outdated or incorrect records, purge test data, enforce data retention policies.
    
- **Precautions:** _Critical_: Without a `WHERE`, `DELETE` removes all rows (equivalent to `TRUNCATE`, but logged and slower). Always double-check the condition. As with UPDATE, it’s wise to run a `SELECT` with the same `WHERE` first, and use a transaction so you can rollback if needed. Alternatively, instead of deleting, consider a “soft delete” (mark as inactive) if historical data may be needed.
    

## Common Pitfalls & Best Practices

- **Avoid `SELECT *` in production:** Select only needed columns. `SELECT *` can fetch unnecessary data, slow queries, and prevent using efficient indexes. It can also lead to bugs if table schemas change. Use column lists or `SELECT DISTINCT col1, col2` for unique rows when needed.
    
- **Always qualify with WHERE for UPDATE/DELETE:** Omitting `WHERE` updates/deletes every row. To prevent mistakes, some recommend writing the `WHERE` first or using transactions. Test your logic by running a `SELECT` with the same `WHERE`.
    
- **Joins:** Specify the join condition clearly with `ON`. Using the wrong join type can exclude needed data or include unwanted NULLs. Watch for Cartesian products (missing `ON` or `WHERE` in a multi-table query). Use aliases to clarify each table’s columns.
    
- **Index usage:** For large tables, ensure columns used in `JOIN`, `WHERE`, or grouping are indexed to speed up queries. For example, if you frequently filter by `last_name`, an index on that column can greatly improve performance.
    
- **Be mindful of NULLs:** SQL treats NULL specially. Use `IS NULL` or `IS NOT NULL` (not `=`) to filter nulls. Remember that aggregate functions ignore NULLs by default (e.g. `AVG`, `SUM` skip nulls) and comparisons with NULL yield false.
    
- **Aggregations:** When grouping, every selected column must be either in `GROUP BY` or an aggregate; otherwise it’s invalid. Also use `HAVING` for conditions on aggregates.
    
- **String comparisons:** Use `LIKE '%pattern%'` for substring matches. Remember that `%` matches any sequence, and `_` matches a single character.
    
- **Testing & formatting:** Write queries cleanly (consistent capitalization, indentation) to avoid mistakes. Comment complex logic. Test queries on small subsets or with `LIMIT`. Use `EXPLAIN` or query plan tools to catch performance issues early.
    
- **Transactions:** For multiple related DML statements (especially UPDATE/DELETE), wrap them in a transaction so you can rollback on error. Always commit or rollback explicitly.
    
- **Data cleanup best practices:** When standardizing data, use functions (e.g. `UPPER()`, `TRIM()`, `REPLACE()`) within an `UPDATE`. Use `DISTINCT` or grouping to detect duplicates. Add constraints or unique indexes to prevent future duplicates.
    

Overall, focus on writing **correct, efficient, and maintainable** SQL. Follow best practices like using descriptive names, optimizing filters, and modularizing complex queries (CTEs or views). Avoid over-complicating queries; simplicity aids both readability and performance.

## Sample Interview Questions & Answers

1. **Q: Write a query to list customers and their orders (if any). Use a join so that customers with no orders still appear.**  
    **A:** Use a **LEFT JOIN** from `Customers` to `Orders`. For example:
    
    ```sql
    SELECT c.customer_id, c.name, o.order_id, o.order_date
      FROM Customers c
      LEFT JOIN Orders o ON c.customer_id = o.customer_id;
    ```
    
    This returns all customers, and `NULL` for order fields if a customer has no orders (because LEFT JOIN includes all left-side rows). If it were an INNER JOIN, customers without orders would be excluded.
    
2. **Q: How do you find products whose total sales exceed 1000?**  
    **A:** Use `GROUP BY` on `product_id`, sum the sales, and filter with `HAVING` (since it’s an aggregate condition):
    
    ```sql
    SELECT product_id, SUM(quantity) AS total_sold
      FROM OrderItems
     GROUP BY product_id
     HAVING SUM(quantity) > 1000;
    ```
    
    This groups rows by product and then filters out groups where the sum is not > 1000. (`WHERE` can’t be used for aggregate filters.)
    
3. **Q: Give an example of a correlated subquery in a SELECT statement.**  
    **A:** A common example is comparing each row to a group-level value. For instance, find employees earning more than their department’s average:
    
    ```sql
    SELECT e.last_name, e.salary, e.department_id
      FROM Employees e
     WHERE e.salary > (
             SELECT AVG(salary)
               FROM Employees
              WHERE department_id = e.department_id
          );
    ```
    
    Here the inner query (`SELECT AVG(salary) …`) depends on `e.department_id` from the outer query. It’s executed for each employee, returning true if that employee’s salary exceeds their dept’s average.
    
4. **Q: How would you correct a misspelled country name in the `Customers` table?**  
    **A:** Use an UPDATE with a WHERE filter. For example, if some rows have `country = 'Cnada'`, standardize to 'Canada':
    
    ```sql
    UPDATE Customers
       SET country = 'Canada'
     WHERE country IN ('Cnada', 'Canda');
    ```
    
    This fixes the typo. Note the `WHERE` ensures only the bad rows are changed. In practice, wrap this in a transaction and perhaps `SELECT` first to verify the affected rows.
    
5. **Q: What’s the difference between `DELETE` and `TRUNCATE`, and when would you use each?**  
    **A:** `DELETE FROM table` removes rows one by one and logs each delete; you can use `WHERE` to delete specific rows (e.g. `DELETE FROM Orders WHERE order_date < '2020-01-01'`). `TRUNCATE TABLE table` (a DDL command) removes _all_ rows without logging individual deletes, resetting the table. Use `DELETE` when you need to filter or preserve transaction control; use `TRUNCATE` for quickly emptying a table (and know that it _cannot_ be rolled back in many databases once committed). In data-analytics, analysts rarely use TRUNCATE, as it’s more a DBA operation.
    
6. **Q: How do you avoid returning duplicate rows in a query?**  
    **A:** Use `SELECT DISTINCT` on the columns of interest. For example, to list unique product categories:
    
    ```sql
    SELECT DISTINCT category
      FROM Products;
    ```
    
    This ensures each category appears once. Alternatively, you can group by the columns:
    
    ```sql
    SELECT category, COUNT(*) 
      FROM Products
     GROUP BY category;
    ```
    
    For just unique values, `DISTINCT` is direct. Also, consider enforcing unique constraints on the table if duplicates shouldn’t exist.
    
7. **Q: How would you give each customer a unique sequential number in the result set?** _(Hint: maybe by rank or row number)_  
    **A:** Use a window function (supported in many SQL dialects). For example, in SQL Server or Postgres:
    
    ```sql
    SELECT ROW_NUMBER() OVER (ORDER BY signup_date) AS customer_rank,
           customer_id, name
      FROM Customers;
    ```
    
    This adds a `customer_rank` column numbering the rows. (Window functions are beyond classic DML, but useful for data analysis tasks like ranking.)
    
8. **Q: Demonstrate updating multiple rows conditionally.**  
    **A:** Suppose we want to increase salaries by 10% for all employees in ‘Sales’:
    
    ```sql
    UPDATE Employees
       SET salary = salary * 1.10
     WHERE department = 'Sales';
    ```
    
    This multiplies the salary by 1.10 for each matching row. Again, check `WHERE` carefully.
    
9. **Q: A table has duplicate entries you want to clean up – how would you remove duplicates?**  
    **A:** One approach is to use a subquery or CTE with ROW_NUMBER():
    
    ```sql
    WITH cte AS (
      SELECT *, ROW_NUMBER() OVER (PARTITION BY col1, col2 ORDER BY id) AS rn
        FROM MyTable
    )
    DELETE FROM cte WHERE rn > 1;
    ```
    
    This keeps the first row of each `(col1, col2)` group and deletes others. Alternatively, copy distinct rows into a new table or use a temporary table to dedupe. Always back up data before bulk deletes.

