

## ✅ What Are Window Functions in SQL?

**Window functions** perform calculations **across a set of rows** related to the current row, **without collapsing the result set**.  
They require an `OVER()` clause that defines the **window (partition + order)**.

---

## 🧱 Basic Syntax

```sql
SELECT column,
       window_function(...) OVER (
           PARTITION BY col1
           ORDER BY col2
       ) AS alias
FROM table;
```

---

## 1. 📊 RANKING FUNCTIONS

### A. `RANK()`, `DENSE_RANK()`, `ROW_NUMBER()`

```sql
SELECT name, department, salary,
       RANK() OVER(PARTITION BY department ORDER BY salary DESC) AS rank,
       DENSE_RANK() OVER(PARTITION BY department ORDER BY salary DESC) AS dense_rank,
       ROW_NUMBER() OVER(PARTITION BY department ORDER BY salary DESC) AS row_num
FROM employees;
```

- **RANK()**: skips ranks on tie
    
- **DENSE_RANK()**: no skips
    
- **ROW_NUMBER()**: unique index per row
    

---

## 2. 💰 CUMULATIVE SUM & AVERAGE

```sql
SELECT name, department, salary,
       SUM(salary) OVER(PARTITION BY department ORDER BY salary) AS cumulative_salary,
       AVG(salary) OVER(PARTITION BY department ORDER BY salary) AS cumulative_avg
FROM employees;
```

---

## 3. 🔁 RUNNING AVERAGE (MOVING WINDOW)

```sql
SELECT name, hire_date, salary,
       AVG(salary) OVER(ORDER BY hire_date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS running_avg
FROM employees;
```

- Uses `ROWS BETWEEN` to define a **moving window**.
    
- Replace `2 PRECEDING` with your desired window size.
    

---

## 4. 📈 PERCENT OF TOTAL

```sql
SELECT name, department, salary,
       salary * 1.0 / SUM(salary) OVER(PARTITION BY department) AS percent_of_total
FROM employees;
```

- Shows each row’s contribution to the total per department.
    

---

## 5. 🔄 PERCENT CHANGE

```sql
SELECT name, hire_date, salary,
       salary - LAG(salary) OVER(ORDER BY hire_date) AS change,
       (salary - LAG(salary) OVER(ORDER BY hire_date)) * 1.0 / LAG(salary) OVER(ORDER BY hire_date) AS percent_change
FROM employees;
```

- Uses `LAG()` to get the previous row’s value for comparison.
    

---

## 6. 🧮 PERCENTILES & QUANTILES

### A. Continuous Percentile:

```sql
SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY salary)
       OVER (PARTITION BY department) AS median_salary
FROM employees;
```

### B. Discrete Percentile:

```sql
SELECT PERCENTILE_DISC(0.25) WITHIN GROUP (ORDER BY salary)
       OVER (PARTITION BY department) AS first_quartile
FROM employees;
```

---

## 7. 🧩 SEGMENTATION / BINNING

### A. NTILE (e.g., Quartiles)

```sql
SELECT name, salary,
       NTILE(4) OVER(ORDER BY salary) AS quartile
FROM employees;
```

- Divides rows into **equal-sized groups**.
    

---

## 8. 📈 CUMULATIVE DISTRIBUTION

```sql
SELECT name, salary,
       CUME_DIST() OVER(ORDER BY salary) AS cumulative_distribution
FROM employees;
```

- Tells the **fraction of rows** with value less than or equal to the current row.
    

---

## 9. 🎯 PARTITIONING BY MULTIPLE COLUMNS

```sql
SELECT name, region, department, salary,
       RANK() OVER(PARTITION BY region, department ORDER BY salary DESC) AS dept_rank
FROM employees;
```

- Partitions data **hierarchically**: first by `region`, then by `department`.
    

---

## ✅ Summary Table

|Function|Purpose|Example Keyword|
|---|---|---|
|`RANK()`|Rank with gaps|`RANK() OVER (...)`|
|`DENSE_RANK()`|Rank without gaps|`DENSE_RANK() OVER (...)`|
|`ROW_NUMBER()`|Unique row number|`ROW_NUMBER() OVER (...)`|
|`SUM()`|Cumulative total|`SUM(...) OVER (...)`|
|`AVG()`|Cumulative average|`AVG(...) OVER (...)`|
|`NTILE(n)`|Percentile buckets|`NTILE(4) OVER (...)`|
|`LAG()/LEAD()`|Previous/next row|`LAG(col) OVER (...)`|
|`PERCENTILE_CONT()`|Continuous percentile|`PERCENTILE_CONT(...)`|
|`PERCENTILE_DISC()`|Discrete percentile|`PERCENTILE_DISC(...)`|
|`CUME_DIST()`|Cumulative distribution|`CUME_DIST() OVER (...)`|
