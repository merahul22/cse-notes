

SQL (Structured Query Language) is used for managing relational databases. Pandas allows interaction with SQL databases using:

- `read_sql()` – read SQL queries.
    
- `read_sql_query()` – specifically for SQL `SELECT` queries.
    
- `read_sql_table()` – read entire SQL table (requires SQLAlchemy).
    

---

### 🔧 Prerequisites

Install required libraries:

```bash
pip install pandas sqlalchemy mysql-connector-python
```

---

### 🔹 Connect to MySQL (via XAMPP)

**1. Start MySQL Server using XAMPP**

**2. Create Database & Table:**

```sql
CREATE DATABASE testdb;

USE testdb;

CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    department VARCHAR(50),
    salary FLOAT
);
```

**3. Insert Data:**

```sql
INSERT INTO employees VALUES
(1, 'Alice', 'IT', 65000),
(2, 'Bob', 'HR', 55000),
(3, 'Charlie', 'Sales', 60000);
```

---

### 🔗 Connect and Read with Pandas

```python
from sqlalchemy import create_engine
import pandas as pd

engine = create_engine('mysql+mysqlconnector://root:@localhost:3306/testdb')

df = pd.read_sql('SELECT * FROM employees', con=engine)
print(df)
```

---

### 📝 Writing DataFrame to SQL

```python
df.to_sql('employees_backup', con=engine, index=False, if_exists='replace')
```

- `if_exists='replace'`: replaces table if it exists.
    
- Use `'append'` to add rows.
    

---

### 📥 Reading in Chunks

```python
chunks = pd.read_sql('SELECT * FROM employees', con=engine, chunksize=2)
for chunk in chunks:
    print(chunk)
```

---

### 🔠 Parameters

|Parameter|Description|
|---|---|
|`sql`|SQL query or table name|
|`con`|Connection object or SQLAlchemy engine|
|`index_col`|Column to use as index|
|`coerce_float`|Convert numeric values|
|`parse_dates`|Automatically parse datetime fields|
|`chunksize`|Load large data in pieces|


JSON (JavaScript Object Notation) is a lightweight data format used commonly in web APIs and configuration files. Pandas makes it easy to import JSON into a DataFrame.

---

### 🔹 Reading JSON from Local File

```python
import pandas as pd

df = pd.read_json('data.json')
print(df)
```

---

### 🔹 Reading JSON from URL

```python
df = pd.read_json('https://api.example.com/data.json')
```

---

### 🔹 Reading JSON Lines Format

```python
df = pd.read_json('logs.json', lines=True)
```

Each line must be a valid JSON object.

---

### 🔹 Handling Nested JSON

When dealing with nested structures, use `json_normalize`.

```python
import json
from pandas import json_normalize

with open('nested.json') as f:
    data = json.load(f)

df = json_normalize(data, record_path=['employees'], meta=['department'])
```

---

### 🔹 Parameters for `read_json()`

|Parameter|Description|
|---|---|
|`path_or_buf`|File path, URL, or JSON string|
|`orient`|Format of JSON ('records', 'index', 'columns')|
|`typ`|Type of object to return: ‘frame’ or ‘series’|
|`dtype`|Data type conversion|
|`convert_dates`|Try to parse dates|
|`lines`|Set to `True` for JSON Lines format|

---

### 📝 JSON Writing

Export DataFrame to JSON:

```python
df.to_json('output.json', orient='records', lines=True)
```

- `orient='records'`: each row as a dictionary.
    
- `lines=True`: makes it JSON Lines format.
    

---

## 🧠 Use Cases

|Task|Preferred Format|
|---|---|
|API data|JSON|
|Database backup|SQL|
|BigQuery/BI integration|SQL|
|Config files & nested records|JSON|
