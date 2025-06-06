

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
