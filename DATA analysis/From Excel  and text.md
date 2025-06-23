

## 📘 **1. Reading Excel Files (`.xlsx`, `.xls`)**

Pandas provides the `read_excel()` function to read Excel files.

### 🔹 **Basic Syntax:**

```python
import pandas as pd

df = pd.read_excel('data.xlsx')
print(df.head())
```

> Requires `openpyxl` for `.xlsx` or `xlrd` for older `.xls` files:

```bash
pip install openpyxl
```

---

### 🔹 **Specifying a Sheet:**

Excel files can contain multiple sheets. You can specify:

- Sheet name
    
- Sheet number (0-indexed)
    

```python
df = pd.read_excel('data.xlsx', sheet_name='Sales')
df2 = pd.read_excel('data.xlsx', sheet_name=0)  # First sheet
```

---

### 🔹 **Reading Multiple Sheets at Once:**

```python
sheets = pd.read_excel('data.xlsx', sheet_name=['Sales', 'Expenses'])
print(sheets['Sales'].head())
```

---

### 🔹 **Other Excel Parameters:**

|Parameter|Purpose|
|---|---|
|`usecols`|Load selected columns|
|`skiprows`|Skip initial rows|
|`nrows`|Read specific number of rows|
|`index_col`|Set column as index|
|`parse_dates`|Parse date columns|
|`na_values`|Define custom missing values|
|`dtype`|Force data types|

---

### 🔹 **Example:**

```python
df = pd.read_excel('data.xlsx', sheet_name='Employees', usecols='A:C', skiprows=1, nrows=10, dtype={'Salary': float})
```

---

## 📄 **2. Reading Plain Text Files**

Plain text files may be:

- Delimited (e.g., CSV, TSV, pipe `|`)
    
- Fixed-width formatted (FWF)
    

---

### 🔹 **A. Reading Delimited Text Files**

Use `read_csv()` with the right `sep`.

```python
df = pd.read_csv('data.txt', sep='\t')        # Tab-delimited
df = pd.read_csv('data.txt', sep='|')         # Pipe-delimited
```

---

### 🔹 **B. Reading Fixed-Width Text Files (`.txt`)**

Use `read_fwf()` when columns are aligned by width, not by delimiter.

```python
df = pd.read_fwf('fixed_width.txt')
```

You can also specify column widths:

```python
df = pd.read_fwf('fixed_width.txt', widths=[10, 15, 20])
```

---

### 🔹 **Example with All Parameters:**

```python
df = pd.read_csv(
    'employees.txt',
    sep='|',
    header=0,
    usecols=['Name', 'Dept', 'Salary'],
    skiprows=1,
    nrows=10,
    dtype={'Salary': float},
    parse_dates=['JoinDate'],
    na_values=['NA', 'N/A']
)
```
