

## 🧠 THEORY: What is EDA?

**Exploratory Data Analysis (EDA)** is the process of examining datasets to:

- Understand their structure and content
    
- Identify data quality issues (missing values, duplicates)
    
- Discover patterns, relationships, and anomalies
    
- Prepare for modeling or further statistical analysis
    

In SQL, EDA focuses on **writing queries to analyze data inside the database**, instead of exporting to Python

---

## 📋 EDA PLAN IN SQL (Step-by-Step)

---

### 🔹 1. **Data Snapshot & Structure**

- **Goal:** Get a quick look at the data shape, size, and sample.
    
- **Tasks:**
    
    - View head/tail/sample rows
        
    - Understand column types & unique counts
        
- **SQL:**
    

```sql
SELECT * FROM table_name LIMIT 5;  -- Head
SELECT * FROM table_name ORDER BY id DESC LIMIT 5;  -- Tail
SELECT COUNT(*) FROM table_name;  -- Row count
SHOW COLUMNS FROM table_name;     -- Structure
```

---

### 🔹 2. **Missing Values Analysis**

- **Goal:** Detect missing or null values in each column.
    
- **Why Important:** Helps clean or impute data later.
    
- **SQL (example):**
    

```sql
SELECT 
  COUNT(*) AS total_rows,
  SUM(CASE WHEN column_name IS NULL THEN 1 ELSE 0 END) AS missing_col
FROM table_name;
```

Repeat for all key columns. For categorical columns, also check `''` (empty string).

---

### 🔹 3. **Duplicate Records**

- **Goal:** Identify and remove duplicate rows.
    
- **SQL:**
    

```sql
SELECT column1, column2, COUNT(*) 
FROM table_name 
GROUP BY column1, column2 
HAVING COUNT(*) > 1;
```

To remove:

```sql
DELETE FROM table_name
WHERE id NOT IN (
  SELECT MIN(id)
  FROM table_name
  GROUP BY column1, column2
);
```

---

### 🔹 4. **Univariate Analysis**

#### ➤ A. **Numerical Columns**

- **Tasks:**
    
    - 8-number summary: min, max, mean, stddev, Q1, median, Q3
        
    - Check for outliers using IQR
        
- **SQL:**
    

```sql
SELECT
  MIN(col), MAX(col), AVG(col), STDDEV(col),
  PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY col) AS Q1,
  PERCENTILE_CONT(0.50) WITHIN GROUP (ORDER BY col) AS Median,
  PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY col) AS Q3
FROM table_name;
```

#### ➤ B. **Categorical Columns**

- **Tasks:**
    
    - Value counts
        
    - % distribution
        
- **SQL:**
    

```sql
SELECT category_col, COUNT(*) AS freq, 
       ROUND(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM table_name), 2) AS percentage
FROM table_name
GROUP BY category_col
ORDER BY freq DESC;
```

---

### 🔹 5. **Bivariate Analysis**

#### ➤ A. **Numerical vs Numerical**

- **Goal:** Check correlation and scatter plot data.
    
- **SQL (correlation):**
    

```sql
SELECT CORR(col1, col2) FROM table_name;
```

- **For scatterplots:** export data: `SELECT col1, col2 FROM table_name`
    

#### ➤ B. **Categorical vs Categorical**

- **Goal:** Contingency tables (pivot-style cross counts)
    
- **SQL:**
    

```sql
SELECT cat1,
  COUNT(CASE WHEN cat2 = 'X' THEN 1 END) AS x_count,
  COUNT(CASE WHEN cat2 = 'Y' THEN 1 END) AS y_count
FROM table_name
GROUP BY cat1;
```

#### ➤ C. **Numerical vs Categorical**

- **Goal:** Compare means or distributions across categories
    
- **SQL:**
    

```sql
SELECT category_col, AVG(numerical_col) AS avg_val
FROM table_name
GROUP BY category_col;
```

---

### 🔹 6. **Outlier Detection (Numerical Data)**

- **Method:** Use IQR rule
    
- **SQL:**
    

```sql
WITH iqr AS (
  SELECT
    PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY price) AS Q1,
    PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY price) AS Q3
  FROM table_name
)
SELECT * FROM table_name
WHERE price < (SELECT Q1 - 1.5*(Q3 - Q1) FROM iqr)
   OR price > (SELECT Q3 + 1.5*(Q3 - Q1) FROM iqr);
```

---

### 🔹 7. **Feature Engineering**

- **Goal:** Derive new columns for modeling
    
- **Examples:**
    
    - `price_per_gb = price / ram`
        
    - `price_bracket` (categorical)
        
- **SQL:**
    

```sql
ALTER TABLE table_name ADD COLUMN price_per_gb INT;

UPDATE table_name
SET price_per_gb = ROUND(price / ram);
```

---

### 🔹 8. **Encoding for Modeling**

#### ➤ One-Hot Encoding

```sql
ALTER TABLE table_name ADD COLUMN is_windows INT;

UPDATE table_name
SET is_windows = CASE WHEN os = 'windows' THEN 1 ELSE 0 END;
```

#### ➤ Label Encoding

```sql
UPDATE table_name
SET os = CASE 
  WHEN os = 'windows' THEN 1
  WHEN os = 'macos' THEN 2
  ELSE 0
END;
```

---

### 🔹 9. **EDA Summary Checks**

- Total rows and columns
    
- % nulls
    
- % duplicates
    
- Most important features by variance
    
- Any transformation applied
    

---

## 🧪 Sample Checklist Before Modeling

|Step|Checked|
|---|---|
|Data loaded properly|✅|
|Column types verified|✅|
|Nulls analyzed/handled|✅|
|Outliers detected|✅|
|Feature engineered|✅|
|Data saved/exported|✅|
