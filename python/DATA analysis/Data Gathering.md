

## **1. Working with CSV Files in Pandas**

CSV (Comma-Separated Values) files are plain text files used to store tabular data. Pandas offers powerful functions like `read_csv()` and `to_csv()` to work with CSVs.

---

## **2. Opening a Local CSV File**

Use `pd.read_csv()` to open a CSV file on your computer.

**Syntax:**

```python
import pandas as pd
df = pd.read_csv('filename.csv')
print(df.head())
```

---

## **3. Opening a CSV File from a URL**

You can directly pass a URL to `read_csv()`.

**Example:**

```python
url = 'https://people.sc.fsu.edu/~jburkardt/data/csv/hw_200.csv'
df = pd.read_csv(url)
print(df.head())
```

---

## **4. `sep` Parameter**

Used to specify the delimiter (default is comma `,`).

**Syntax:**

```python
df = pd.read_csv('data.tsv', sep='\t')  # For tab-separated file
```

---

## **5. `index_col` Parameter**

Use a column as the index instead of default 0-based indexing.

**Syntax:**

```python
df = pd.read_csv('file.csv', index_col='ID')
```

---

## **6. `header` Parameter**

Specify which row to use as the header (0-indexed). Use `None` if there is no header.

**Example:**

```python
df = pd.read_csv('file.csv', header=1)  # Second row as header
```

---

## **7. `usecols` Parameter**

Load only selected columns.

**Example:**

```python
df = pd.read_csv('file.csv', usecols=['Name', 'Age'])
```

---

## **8. `squeeze` Parameter** _(Deprecated in newer versions)_

If the parsed data has only one column, returns a Series instead of DataFrame.

**Example (older versions):**

```python
series = pd.read_csv('single_col.csv', squeeze=True)
```

---

## **9. `skiprows` / `nrows` Parameters**

- `skiprows`: Skip initial lines.
    
- `nrows`: Read only specified number of rows.
    

**Example:**

```python
df = pd.read_csv('file.csv', skiprows=2, nrows=5)
```

---

## **10. `encoding` Parameter**

Useful for reading files with special characters.

**Example:**

```python
df = pd.read_csv('file.csv', encoding='utf-8')       # Default
df = pd.read_csv('file.csv', encoding='latin1')      # For accented chars
```

---

## **11. `on_bad_lines` Parameter** _(Pandas ≥1.3.0)_

Skip lines with too many fields or errors.

**Example:**

```python
df = pd.read_csv('bad.csv', on_bad_lines='skip')
```

---

## **12. `dtype` Parameter**

Define data types explicitly for columns.

**Example:**

```python
df = pd.read_csv('file.csv', dtype={'Age': int, 'Name': str})
```

---

## **13. Handling Dates – `parse_dates`**

Convert date columns to `datetime` objects automatically.

**Example:**

```python
df = pd.read_csv('sales.csv', parse_dates=['Date'])
```

---

## **14. `converters` Parameter**

Apply functions to values during reading.

**Example:**

```python
df = pd.read_csv('file.csv', converters={'Price': lambda x: float(x.strip('$'))})
```

---

## **15. `na_values` Parameter**

Specify custom missing values.

**Example:**

```python
df = pd.read_csv('file.csv', na_values=['NA', '-', 'N/A'])
```

---

## **16. Loading Huge Datasets in Chunks**

Use `chunksize` to process data in manageable parts.

**Example:**

```python
chunks = pd.read_csv('bigdata.csv', chunksize=1000)
for chunk in chunks:
    print(chunk.shape)
```
