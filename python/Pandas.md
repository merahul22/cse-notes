

## 1. What is a Pandas Series?

A **Pandas Series** is a one-dimensional array-like object containing an array of data and an associated array of data labels (index). It is built on top of NumPy and offers enhanced functionality.

### Theory:

- It allows operations similar to both a list and a dictionary.
    
- Supports vectorized operations which make it fast and efficient for numerical computations.
    

### Use Cases:

- Represent a single column from a DataFrame.
    
- Perform statistical analysis on a single variable.
    
- Simplify element-wise operations on data.
    

---

## 2. Importing Pandas

```python
import pandas as pd
```

> Required to access all pandas functionalities.

---

## 3. Creating Series

### a. From a List

```python
pd.Series([10, 20, 30])
```

> Creates a default-indexed Series from a list.

### b. With Custom Index

```python
pd.Series([10, 20, 30], index=['a', 'b', 'c'])
```

> Allows meaningful indexing using custom labels.

### c. From Dictionary

```python
pd.Series({'math': 90, 'eng': 80})
```

> Automatically assigns keys as index labels.

### d. From CSV (1D column)

```python
df = pd.read_csv('file.csv')
s = df.squeeze()
```

> `squeeze()` converts single-column DataFrame into Series.

---

## 4. Series Attributes

### Theory:

Attributes provide meta-information about Series.

```python
s.index      # Labels
s.values     # Numpy array
s.dtype      # Data type
s.name       # Optional name
s.shape      # Tuple form
s.ndim       # Always 1 for Series
```

---

## 5. Viewing Data

### Theory:

Used for inspecting parts of a Series.

```python
s.head()        # First 5 values
s.tail(3)       # Last 3 values
s.sample(2)     # Random values
s.unique()      # Unique elements
s.nunique()     # Number of unique values
```

---

## 6. Frequency Counts

```python
s.value_counts()
```

> Counts frequency of unique elements.

```python
s.value_counts(normalize=True)
```

> Returns relative frequencies (percentages).

---

## 7. Sorting

```python
s.sort_values()             # By values
s.sort_values(ascending=False)
s.sort_index()              # By index
```

> Useful in organizing or analyzing data.

---

## 8. Math & Stats

```python
s.sum(), s.mean(), s.min(), s.max()
s.std(), s.median(), s.describe()
```

> Fast summary statistics to explore distribution.

---

## 9. Indexing & Slicing

```python
s[0]             # Position
s['math']        # Label
s[1:3]           # Slice
s[-1]            # Negative indexing
s[['math', 'eng']]   # Fancy indexing
```

> Powerful access to subsets of data.

---

## 10. Editing Series

```python
s['math'] = 100
s.iloc[0] = 999
```

> Supports both label and positional modification.

---

## 11. Copy vs View

```python
s2 = s.copy()    # New memory
s3 = s           # Same memory
```

> Important for memory efficiency and avoiding bugs.

---

## 12. Functional Operations

```python
s.apply(lambda x: x*2)
s.map(lambda x: x+1)
```

> Apply custom functions to each element.

---

## 13. Arithmetic Operations

```python
s + 5, s * 2
s1.add(s2, fill_value=0)
```

> Vectorized arithmetic with alignment.

---

## 14. Relational & Logical Operations

```python
s > 10
(s > 5) & (s < 15)
```

> Returns boolean Series.

---

## 15. Boolean Indexing

```python
s[s > 50]
s[s.isin([60, 70])]
```

> Filter data based on conditions.

---

## 16. Plotting Series

```python
import matplotlib.pyplot as plt
s.plot(kind='bar')
plt.show()
```

> Visualizes data for easier interpretation.

---

## 17. Other Important Methods

```python
s.astype(int)                 # Convert dtype
s.between(30, 100)            # Range filter
s.clip(0, 100)                # Limit values
s.drop_duplicates()
s.duplicated()
s.isnull()
s.dropna()
s.fillna(0)
s.isin([100, 200])
s.cumsum(), s.cumprod()
s.idxmax(), s.idxmin()
s.to_list()
```

> Offers data cleaning, conversion, and transformation tools.

---

## Summary & Best Practices

- Use `Series` for 1D labeled data.
    
- Always know your index – it drives alignment.
    
- Prefer `.copy()` if modifying a slice.
    
- Use `.apply()` for custom logic and `.map()` for transformation.
    
- Plotting provides quick insights.


## 📘 **1. What is a DataFrame?**

A `DataFrame` is a two-dimensional, size-mutable, tabular data structure with labeled axes (rows and columns). It's similar to a spreadsheet or SQL table.

```python
import pandas as pd
```

---

## 📗 **2. Creating DataFrames**

### **a. From a List of Lists**

```python
data = [[10, 20], [30, 40], [50, 60]]
df = pd.DataFrame(data, columns=["Maths", "Science"])
print(df)
```

```
   Maths  Science
0     10       20
1     30       40
2     50       60
```

### **b. From a Dictionary**

```python
data = {
    "Name": ["Alice", "Bob", "Charlie"],
    "Marks": [85, 90, 78]
}
df = pd.DataFrame(data)
print(df)
```

### **c. From a CSV File**

```python
df = pd.read_csv("data.csv")  # Assumes file is in working directory
print(df.head())
```

---

## 📘 **3. DataFrame Attributes and Methods**

### **a. `.shape` – Dimensions**

```python
print(df.shape)  # (rows, columns)
```

### **b. `.dtypes` – Data types**

```python
print(df.dtypes)
```

### **c. `.index`, `.columns`, `.values`**

```python
print(df.index)
print(df.columns)
print(df.values)
```

### **d. `.head()`, `.tail()`**

```python
print(df.head())       # First 5 rows
print(df.tail(3))      # Last 3 rows
```

### **e. `.sample()` – Random sample**

```python
print(df.sample(2))  # Random 2 rows
```

### **f. `.info()`**

```python
df.info()
```

### **g. `.describe()`**

```python
print(df.describe())
```

### **h. `.isnull()`, `.sum()` – Missing values**

```python
print(df.isnull())
print(df.isnull().sum())  # Total missing in each column
```

### **i. `.duplicated()`**

```python
print(df.duplicated())
```

### **j. `.rename()` with `inplace`**

```python
df.rename(columns={"Marks": "Scores"}, inplace=True)
```

---

## 📙 **4. Mathematical Methods**

### **a. `.sum()`**

```python
print(df["Scores"].sum())
```

### **b. `.mean()`**

```python
print(df["Scores"].mean())
```

### **c. `.var()` – Variance**

```python
print(df["Scores"].var())
```

### **d. `axis` Argument**

- `axis=0`: column-wise operation (default)
    
- `axis=1`: row-wise operation
    

```python
print(df.sum(axis=0))  # Sum of each column
```

---

## 📒 **5. Selecting Data from DataFrames**

### **a. Single Column**

```python
print(df["Name"])
```

### **b. Multiple Columns**

```python
print(df[["Name", "Scores"]])
```

### **c. Single Row (by position)**

```python
print(df.iloc[1])
```

### **d. Multiple Rows**

```python
print(df.iloc[1:3])
```

---

## 📕 **6. Selecting Rows with `iloc` and `loc`**

### **a. Using `iloc` (position-based)**

```python
print(df.iloc[0])       # First row
print(df.iloc[0:2])     # First two rows
```

### **b. Using `loc` (label-based)**

```python
print(df.loc[0])        # Row with index label 0
```

---

## 📘 **7. Fancy Indexing (Rows + Columns)**

```python
print(df.loc[0:2, ["Name", "Scores"]])
```

---

## 📗 **8. Filtering DataFrames**

### Syntax:

```python
df[condition]
```

### **Example 1: Students with marks > 80**

```python
print(df[df["Scores"] > 80])
```

### **Example 2: Names starting with "A"**

```python
print(df[df["Name"].str.startswith("A")])
```

### **Example 3: Multiple conditions**

```python
print(df[(df["Scores"] > 80) & (df["Scores"] < 90)])
```

---

## 🧪 **Practice Questions for Filtering**

### **Q1: Find students who scored less than 85**

```python
print(df[df["Scores"] < 85])
```

### **Q2: Find students whose name is either 'Alice' or 'Bob'**

```python
print(df[df["Name"].isin(["Alice", "Bob"])])
```

### **Q3: Find students whose scores are not null**

```python
print(df[df["Scores"].notnull()])
```

---

## 🧩 **9. Adding New Columns**

### **a. Adding a Completely New Column**

```python
df["Age"] = [18, 19, 20]
```

### **b. Creating from Existing Columns**

```python
df["Passed"] = df["Scores"] > 80
```

---

## 📝 Summary Table

|Function / Attribute|Description|
|---|---|
|`df.shape`|Returns dimensions|
|`df.dtypes`|Data types of columns|
|`df.head()`|First 5 rows|
|`df.tail()`|Last 5 rows|
|`df.info()`|Summary of the DataFrame|
|`df.describe()`|Statistics of numeric columns|
|`df.isnull()`|Checks for missing values|
|`df.sum()`, `df.mean()`|Math operations|
|`df["col"]`|Select column|
|`df.iloc`, `df.loc`|Select rows (positional vs label)|
|`df[condition]`|Filtering|
|`df["new_col"] = ...`|Adding new column|


## 📘 **1. `value_counts()`**

**Use:** Returns a Series with counts of unique values.

```python
df["Scores"].value_counts()
```

📝 _Great for counting frequencies in a column._

---

## 📘 **2. `sort_values()`**

**Use:** Sorts values by a column.

```python
df.sort_values(by="Scores", ascending=False)
```

---

## 📘 **3. `rank()`**

**Use:** Assigns ranks to values (1 being the lowest by default).

```python
df["Rank"] = df["Scores"].rank(ascending=False)
```

---

## 📘 **4. `sort_index()`**

**Use:** Sorts the DataFrame by index (rows).

```python
df.sort_index()
```

---

## 📘 **5. `set_index()`**

**Use:** Sets a column as the new row index.

```python
df2 = df.set_index("Name")
```

---

## 📘 **6. `rename()` for Index or Columns**

**Use:** Renames index or column labels.

```python
df.rename(columns={"Scores": "Marks"}, inplace=True)
df.rename(index={0: "Student1"}, inplace=True)
```

---

## 📘 **7. `reset_index()`**

**Use:** Resets the index to default (useful after `set_index()`).

```python
df2.reset_index(inplace=True)
```

---

## 📘 **8. `unique()` and `nunique()`**

**Use:**

- `unique()` – returns unique values
    
- `nunique()` – count of unique values
    

```python
df["Name"].unique()
df["Name"].nunique()
```

---

## 📘 **9. `isnull()` / `notnull()` / `hasnans`**

```python
df.isnull()
df.notnull()
df["Scores"].hasnans  # Boolean check
```

---

## 📘 **10. `dropna()`**

**Use:** Drops missing values.

```python
df.dropna()  # Removes rows with NaN
df.dropna(axis=1)  # Removes columns with NaN
```

---

## 📘 **11. `fillna()`**

**Use:** Fills missing values.

```python
df.fillna(0)
df["Scores"].fillna(df["Scores"].mean())
```

---

## 📘 **12. `drop_duplicates()`**

**Use:** Removes duplicate rows.

```python
df.drop_duplicates()
```

---

## 📘 **13. `drop()`**

**Use:** Drops rows or columns by label.

```python
df.drop("Age", axis=1)        # Drop column
df.drop([0, 2], axis=0)       # Drop rows by index
```

---

## 📘 **14. `apply()`**

**Use:** Applies a function to each row/column.

```python
df["Double"] = df["Scores"].apply(lambda x: x * 2)
```

---

## 📘 **15. `isin()`**

**Use:** Filters rows based on whether column value is in a list.

```python
df[df["Name"].isin(["Alice", "Bob"])]
```

---

## 📘 **16. `corr()`**

**Use:** Returns correlation between numeric columns.

```python
df.corr()
```

---

## 📘 **17. `nlargest()` / `nsmallest()`**

**Use:** Returns top/bottom N rows based on a column.

```python
df.nlargest(2, "Scores")
df.nsmallest(2, "Scores")
```

---

## 📘 **18. `insert()`**

**Use:** Insert a new column at specific position.

```python
df.insert(1, "Grade", ["A", "B", "C"])
```

---

## 📘 **19. `copy()`**

**Use:** Creates a copy of the DataFrame (important to avoid modifying original data).

```python
df_copy = df.copy()
```

---

## ✅ **Summary Table**

|Method|Purpose|
|---|---|
|`value_counts()`|Count unique values|
|`sort_values()`|Sort by column values|
|`rank()`|Assign ranking|
|`sort_index()`|Sort by index|
|`set_index()`|Use a column as index|
|`rename()`|Rename columns or index|
|`reset_index()`|Reset index to default|
|`unique()`, `nunique()`|Unique values/count|
|`isnull()` / `notnull()` / `hasnans`|Check for nulls|
|`dropna()` / `fillna()`|Handle missing values|
|`drop_duplicates()`|Remove duplicate rows|
|`drop()`|Remove columns/rows|
|`apply()`|Apply custom function|
|`isin()`|Filter with list of values|
|`corr()`|Correlation matrix|
|`nlargest()` / `nsmallest()`|Top or bottom N values|
|`insert()`|Add column at position|
|`copy()`|Deep copy of DataFrame|
