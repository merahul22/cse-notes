## Exporting Data with Pandas: CSV, Excel, HTML, JSON, and SQL (XAMPP)
---
### 1. Export to CSV

**Function**: `DataFrame.to_csv()`

**Theory**: CSV (Comma-Separated Values) is a universal plain-text format for tabular data.

**Key Parameters:**

- `path_or_buf`: file path or buffer; if `None`, returns as string
    
- `sep`: delimiter (default `,`)
    
- `index`: include row labels (default `True`)
    
- `header`: include column names (default `True`)
    
- `columns`: subset of columns to write
    
- `encoding`: file encoding (e.g., `utf-8`)
    
- `na_rep`: representation for missing data
    
- `mode`: file mode (e.g., `w`, `a`)
    
- `compression`: compression format (`gzip`, `bz2`, etc.)
    

**Syntax:**

```python
DataFrame.to_csv(
    path_or_buf, sep=',', index=True, header=True,
    columns=None, encoding=None, na_rep='',
    mode='w', compression=None
)
```

**Example:**

```python
import pandas as pd

df = pd.DataFrame({'Name': ['Alice', 'Bob'], 'Age': [25, 30]})
# Export with custom settings
df.to_csv('output.csv', sep=';', index=False, na_rep='NA', encoding='utf-8')
```

---

### 2. Export to Excel

**Function**: `DataFrame.to_excel()`

**Theory**: Excel files (`.xlsx`, `.xls`) are widely used for spreadsheets and support multiple sheets.

**Key Parameters:**

- `excel_writer`: file path or `ExcelWriter` object
    
- `sheet_name`: name of sheet (default 'Sheet1')
    
- `index`: include row labels
    
- `header`: include column names
    
- `columns`: subset of columns
    
- `engine`: writing engine (`openpyxl`, `xlsxwriter`)
    
- `na_rep`: missing data representation
    
- `startrow`, `startcol`: location to write DataFrame
    
- `freeze_panes`: tuple to freeze rows/cols
    

**Syntax:**

```python
DataFrame.to_excel(
    excel_writer, sheet_name='Sheet1', index=True, header=True,
    columns=None, engine=None, na_rep='',
    startrow=0, startcol=0, freeze_panes=None
)
```

**Example:**

```python
import pandas as pd

df = pd.DataFrame({'Product': ['A', 'B'], 'Price': [100, 150]})
# Write to Excel with formatting options
df.to_excel(
    'products.xlsx', sheet_name='Inventory', index=False,
    na_rep='N/A', startrow=1, startcol=1
)
```

---

### 3. Export to HTML

**Function**: `DataFrame.to_html()`

**Theory**: HTML tables embed DataFrames into webpages or reports.

**Key Parameters:**

- `buf`: file path or buffer
    
- `columns`: subset of columns
    
- `index`: include row labels
    
- `border`: table border size
    
- `na_rep`: missing data representation
    
- `justify`: column alignment (`'left'`, `'right'`, `'center'`)
    
- `classes`: CSS classes to add
    
- `table_id`: HTML `id` attribute
    

**Syntax:**

```python
DataFrame.to_html(
    buf=None, columns=None, index=True,
    border=None, na_rep='NaN', justify='right',
    classes=None, table_id=None
)
```

**Example:**

```python
import pandas as pd

df = pd.DataFrame({'City': ['X', 'Y'], 'Temp': [22, 25]})
# Export to HTML with custom CSS class
df.to_html('weather.html', index=False, classes='table table-striped')
```

---

### 4. Export to JSON

**Function**: `DataFrame.to_json()`

**Theory**: JSON is a lightweight data-interchange format ideal for web APIs.

**Key Parameters:**

- `path_or_buf`: file path or buffer
    
- `orient`: format of JSON (`'records'`, `'split'`, `'index'`, `'columns'`, `'table'`)
    
- `date_format`: format for dates (`'epoch'`, `'iso'`)
    
- `double_precision`: decimal precision
    
- `force_ascii`: escape non-ASCII
    
- `date_unit`: time unit (`'ms'`, `'s'`)
    
- `lines`: write JSON Lines if `True`
    
- `compression`: compression format
    

**Syntax:**

```python
DataFrame.to_json(
    path_or_buf=None, orient=None, date_format=None,
    double_precision=10, force_ascii=True,
    date_unit='ms', default_handler=None,
    lines=False, compression=None
)
```

**Example:**

```python
import pandas as pd

df = pd.DataFrame({'id': [1, 2], 'value': [3.14, None]})
# Export as JSON Lines
json_str = df.to_json(orient='records', lines=True)
print(json_str)
# Write to file with compression
df.to_json('data.json.gz', compression='gzip', orient='table')
```

---

### 5. Export to SQL (XAMPP MySQL)

**Function**: `DataFrame.to_sql()`

**Theory**: Save DataFrame into a SQL database table. Using XAMPP means connecting to a local MySQL/MariaDB server.

**Prerequisites:**

```bash
pip install pandas sqlalchemy mysql-connector-python
```

Start MySQL in XAMPP control panel.

**Key Parameters:**

- `name`: target table name
    
- `con`: SQLAlchemy engine or DBAPI connection
    
- `if_exists`: `'fail'`, `'replace'`, or `'append'`
    
- `index`: write DataFrame index as column
    
- `index_label`: label for index column
    
- `schema`: DB schema (if applicable)
    
- `chunksize`: rows per batch
    
- `dtype`: dict of column SQL types
    

**Syntax:**

```python
DataFrame.to_sql(
    name, con, if_exists='fail', index=True,
    index_label=None, schema=None, chunksize=None,
    dtype=None
)
```

**Example:**

```python
import pandas as pd
from sqlalchemy import create_engine

# Connect to XAMPP MySQL
engine = create_engine('mysql+mysqlconnector://root:@localhost:3306/testdb')

df = pd.DataFrame({
    'id': [1,2,3],
    'name': ['A','B','C'],
    'score': [88, 92, 95]
})
# Write DataFrame to SQL table
df.to_sql('scores', con=engine, if_exists='replace', index=False,
            dtype={'id': 'INT', 'name': 'VARCHAR(20)', 'score': 'FLOAT'})
```
