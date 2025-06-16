

**Definition:** Window functions perform calculations across a set (“window”) of rows related to the current row. Unlike normal aggregates, they do **not collapse** rows into one result; instead they return a value for every row while allowing group-like operations. For example, `AVG(Salary) OVER (PARTITION BY Dept)` computes the average in each department but still returns one row per employee.

**Difference from Aggregate Functions:** Aggregate functions (with `GROUP BY`) collapse multiple rows into a single output row per group. Window functions retain each row and add extra computed columns. This means you can combine row-level data with group-level calculations (e.g. list every employee **and** their department’s average salary). Aggregates reduce the number of rows; window functions preserve rows and simply augment them with summary data.

## OVER() Clause Syntax

Window functions use the `OVER()` clause to define the window. The key subclauses are:

- **`PARTITION BY <cols>`:** Splits rows into groups. Each partition is processed independently (like a GROUP BY, but rows are not collapsed).
    
- **`ORDER BY <cols>`:** Sorts rows within each partition. Necessary for functions like `ROW_NUMBER()`, `LAG()`, etc.
    
- **`ROWS` / `RANGE`:** Defines the frame (subset) of rows to use (upper/lower bounds). By default, if `ROWS/RANGE` is omitted and ORDER BY is present, the frame runs from the first row in the partition up to the current row. Specifying `ROWS BETWEEN ...` can change this.
    

All subclauses of `OVER()` are optional. Omitting `PARTITION BY` treats the entire result set as one window; omitting `ORDER BY` means no particular order is assumed and frame defaults to “whole partition.” In summary:

```sql
SELECT ..., 
       window_func(col) OVER (
           [PARTITION BY partition_cols] 
           [ORDER BY sort_cols] 
           [ROWS|RANGE frame_clause]
       ) AS new_col
FROM table;
```

The `OVER` clause controls which rows belong to the window for each output row. For example, `AVG(Salary) OVER (PARTITION BY Department)` computes each department’s average, and `ROW_NUMBER() OVER (ORDER BY HireDate)` numbers all rows by hire date regardless of partitioning.

## Key Window Functions

### Ranking Functions (ROW_NUMBER, RANK, DENSE_RANK)

- **`ROW_NUMBER()`**: Assigns a unique sequential number to each row within its partition, starting at 1. Ties **do not** get the same number – every row is distinct.
    
    ```sql
    SELECT Name, Department, Salary,
           ROW_NUMBER() OVER (PARTITION BY Department ORDER BY Salary DESC) AS row_num
    FROM employee;
    ```
    
    |Name|Department|Salary|row_num|
    |:-:|:-:|:-:|:-:|
    |Ramesh|Finance|50000|1|
    |Suresh|Finance|50000|2|
    |Ram|Finance|20000|3|
    |Deep|Sales|30000|1|
    |Pradeep|Sales|20000|2|
    |Here, each employee gets a unique rank in their department by salary, with no ties sharing a number.||||
    
- **`RANK()`**: Assigns the same rank to tied rows, then skips ranks. Ties get identical ranks but leave gaps. For example, if two rows tie for rank 1, the next row is rank 3.  
      
    _Table: Output of `RANK()` over the sample employees by department._  
    In this example, Ramesh and Suresh (both $50k) tie for rank 1; the next Finance salary (20k) is rank 3. (The next rank after a tie is the previous rank plus the number of tied rows.)
    
- **`DENSE_RANK()`**: Similar to `RANK()`, but does **not** skip ranks. Tied rows share a rank, but the next rank value is consecutive. E.g., two rows tie for rank 1, the next rank is 2. Example:
    
    ```sql
    SELECT Name, Department, Salary,
           DENSE_RANK() OVER (PARTITION BY Department ORDER BY Salary DESC) AS dense_rank
    FROM employee;
    ```
    
    |Name|Department|Salary|dense_rank|
    |:-:|:-:|:-:|:-:|
    |Ramesh|Finance|50000|1|
    |Suresh|Finance|50000|1|
    |Ram|Finance|20000|2|
    |Deep|Sales|30000|1|
    |Pradeep|Sales|20000|2|
    |Here Deep (30k) is rank 1 in Sales, Pradeep (20k) is rank 2. In Finance, Ramesh/Suresh share rank 1, and Ram is rank 2 (no gap).||||
    

### NTILE

- **`NTILE(n)`**: Distributes rows in an ordered partition into _n_ roughly equal buckets (groups) and returns the bucket number for each row. Groups differ in size by at most one. For example, with IDs 1–10:
    
    ```sql
    SELECT ID, NTILE(3) OVER (ORDER BY ID) AS group_num
    FROM geeks_demo;
    ```
    
    |ID|group_num|
    |:-:|:-:|
    |1|1|
    |2|1|
    |3|1|
    |4|1|
    |5|2|
    |6|2|
    |7|2|
    |8|3|
    |9|3|
    |10|3|
    |Here rows 1–4 are in group 1, 5–7 in 2, and 8–10 in 3 (10 rows divided into 3 buckets).||
    

### Lead and Lag (Accessing Other Rows)

- **`LAG(col, N)`** and **`LEAD(col, N)`**: Return the value of a column from **N rows before or after** the current row, respectively (within the same partition and order). If no such row exists, a default (NULL or specified) is returned. These are useful for comparing a row to its predecessor or successor. Example:
    
    ```sql
    SELECT Day, Sales,
           LAG(Sales,1) OVER (ORDER BY Day) AS prev_sales,
           LEAD(Sales,1) OVER (ORDER BY Day) AS next_sales
    FROM daily_sales;
    ```
    
    |Day|Sales|prev_sales|next_sales|
    |:-:|:-:|:-:|:-:|
    |1|100|_NULL_|150|
    |2|150|100|200|
    |3|200|150|_NULL_|
    |This shows each day’s sales, the previous day’s sales (using `LAG`), and the next day’s sales (`LEAD`). For Day=1, `prev_sales` is NULL since no prior row exists, and for Day=3 `next_sales` is NULL.||||
    

### FIRST_VALUE and LAST_VALUE

- **`FIRST_VALUE(col)`** and **`LAST_VALUE(col)`**: Return the first or last value in the window frame (as defined by `ORDER BY`) for each row. For example, on an ordered table:
    
    ```sql
    SELECT row, value,
           FIRST_VALUE(value) OVER (ORDER BY row) AS first_val,
           LAST_VALUE(value)  OVER (
               ORDER BY row
               ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
           ) AS last_val
    FROM T;
    ```
    
    |row|value|first_val|last_val|
    |:-:|:-:|:-:|:-:|
    |1|100|100|300|
    |2|200|100|300|
    |3|300|100|300|
    |Here the window is all rows in ascending order. `FIRST_VALUE(value)` is always 100 (first row’s value) and `LAST_VALUE(value)` is 300 (last row’s value) for every output row. (Note: To get the true last value, we explicitly set `ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING`; otherwise the default frame “to current row” would make LAST_VALUE act like FIRST_VALUE of the reversed order.)||||
    

### Aggregate Functions as Windows (SUM, AVG, MIN, MAX, etc.)

Aggregate functions can be used as window functions with `OVER`. Common examples include:

- `SUM(col) OVER (PARTITION BY ...)`: running total or subtotal.
    
- `AVG(col) OVER (PARTITION BY ...)`: rolling average.
    
- `MIN(col) / MAX(col) OVER (...)`: lowest/highest in window.
    

For example, to show each employee’s salary **and** their department’s average salary:

```sql
SELECT Name, Department, Salary,
       AVG(Salary) OVER (PARTITION BY Department) AS avg_dept_salary
FROM employee;
```

  
_Table: `AVG(Salary) OVER (PARTITION BY Department)` adds each department’s average salary (shown in “Avg_Salary”) to every row._

Another example: compute each department’s total salary with `SUM(...)`:

```sql
SELECT Name, Department, Salary,
       SUM(Salary) OVER (PARTITION BY Department) AS total_dept_salary
FROM employee;
```

|Name|Department|Salary|total_dept_salary|
|:-:|:-:|:-:|:-:|
|Ramesh|Finance|50000|120000|
|Suresh|Finance|50000|120000|
|Ram|Finance|20000|120000|
|Deep|Sales|30000|50000|
|Pradeep|Sales|20000|50000|

Here each row shows the department total ($120k or $50k) alongside the individual salary. The aggregate runs per partition but does not collapse rows.

## Advanced Topics

- **Window Frame Specification:** By default, with `ORDER BY` and no `ROWS/RANGE`, the frame is “all rows from the first row of the partition up to the current row”. You can explicitly control this with `ROWS BETWEEN` or `RANGE BETWEEN`. Common options include `UNBOUNDED PRECEDING` (start of partition), `CURRENT ROW`, or a fixed offset (e.g. `3 PRECEDING`). For example:
    
    ```sql
    SELECT Day, Sales,
           SUM(Sales) OVER (
             ORDER BY Day 
             ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
           ) AS running_total
    FROM daily_sales;
    ```
    
    This calculates a running total of Sales from the start through the current row. In general, `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` defines the frame to include all prior rows. (Without `ROWS/RANGE`, the default frame with `ORDER BY` is the same: from start up to current.) The markers `UNBOUNDED PRECEDING`, `CURRENT ROW`, and `UNBOUNDED FOLLOWING` specify frame boundaries.
    
- **Combining with CTEs/Subqueries:** Window functions can be used in subqueries or CTEs to filter or aggregate further. A common pattern is computing a window function in a subquery, then selecting/filtering from it. For example, to get the top 3 salaries per department:
    
    ```sql
    WITH RankedEmployees AS (
        SELECT Name, Department, Salary,
               RANK() OVER (
                 PARTITION BY Department 
                 ORDER BY Salary DESC
               ) AS dept_rank
        FROM employee
    )
    SELECT Name, Department, Salary
    FROM RankedEmployees
    WHERE dept_rank <= 3;
    ```
    
    This CTE ranks each employee within their department, then the outer query filters to keep only the top 3 (`dept_rank <= 3`). Such use of CTEs or derived tables is common when you need to apply a window function and then further process or filter its results.
    
- **Nested Window Functions:** The SQL standard (SQL:2016) defines “nested window functions” (e.g., applying a window function inside another), but this is not widely supported in practice. In most SQL dialects (like SQL Server’s T-SQL), you cannot directly nest one window inside another; instead you must use subqueries/CTEs to chain operations. For example, to rank the results of a window function, you’d typically put the first window in a CTE and then apply another window to its output.
    

## Performance Tips

Window functions are powerful but can be costly. Key points for efficiency:

- **Sorting and Memory:** Window operations often require sorting partitions (`ORDER BY`) and holding data in memory. On large datasets, this can be expensive. Always ensure the columns you partition or order by are indexed if possible.
    
- **Avoid Unnecessary Windows:** If you only need a simple group total, an ordinary `GROUP BY` may be faster (since it reduces rows). Use a window function when you need row-level detail plus a summary.
    
- **Frame Limits:** Narrow the frame if possible. For example, if you only need the last 3 rows or up to current, explicitly use `ROWS BETWEEN ...` to avoid processing the full partition.
    
- **Alternatives:** Some tasks doable with windows (like “rank within group”) can alternatively be done with subqueries or self-joins, but those are often more complex and slower. In general, a well-indexed window query will perform better for sliding-window analytics. Still, on very large data, consider pre-aggregating or using summary tables if real-time window calculations become a bottleneck.
    
- **Use Cases:** Window functions are ideal for running totals, moving averages, rankings, gap analysis, and top-N queries. For purely aggregate summaries (no need to keep each row), `GROUP BY` is simpler.
    

## Common Interview Questions & Challenges

- **What’s the difference between window functions and aggregate functions?** (Answer: aggregates collapse rows into one per group; window functions return results per row.)
    
- **How do you find the nth highest salary per department?**  
    _Hint:_ Use `ROW_NUMBER()` or `DENSE_RANK()` partitioned by department (ordering by salary descending), then filter on rank = n.
    
- **How to compute a running total or cumulative sum?**  
    _Hint:_ Use `SUM(...) OVER (ORDER BY date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)`.
    
- **How to calculate day-over-day differences or detect gaps?**  
    _Hint:_ Use `LAG()` or `LEAD()` to compare a row to its predecessor or successor.
    
- **What is the difference between `RANK()` and `DENSE_RANK()`?**  
    _Answer:_ `RANK()` skips numbers when ties occur (e.g. ranks 1,1,3), while `DENSE_RANK()` does not skip (e.g. ranks 1,1,2).
    
- **When do you need a framing clause (`ROWS`/`RANGE`)?**  
    _Hint:_ When you want, say, a sliding window of fixed size, or explicitly include/exclude the current row. For example, `RANGE BETWEEN INTERVAL '7 DAY' PRECEDING` for a moving 7-day window.
    
- **Performance challenge:** Given a window query on a large table that’s slow, what can you do?  
    _Hint:_ Check indexes on partition/order columns, limit partition sizes, or consider materializing intermediate results (CTEs or temp tables). Ensure the window frame isn’t larger than needed.
    
- **Nested windows:** Can you apply two window functions in one `SELECT`?  
    _Answer:_ You can put multiple window functions in the same query (they each use the same or different windows), but you cannot nest one inside another without a subquery. Each window function has its own `OVER()` clause.
