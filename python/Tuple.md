

## 🔹 **What is a Tuple?**

A **tuple** in Python is similar to a list, but it is **immutable** — meaning **once created, its elements cannot be changed**.

> ✅ Tuples are faster and more memory-efficient than lists.

---

## 📌 **Characteristics of Tuples**

|Property|Description|
|---|---|
|✅ **Ordered**|Elements have a defined order|
|✅ **Immutable**|Cannot change, add, or delete items|
|✅ **Allows duplicates**|Elements can repeat|
|✅ **Heterogeneous**|Can contain mixed data types|

---

## 🧱 **Creating a Tuple**

```python
t1 = (1, 2, 3)
t2 = ("a", "b", "c")
t3 = (1,)         # Single-element tuple needs a trailing comma
t4 = tuple([4, 5])  # Using constructor
```

---

## 🔍 **Accessing Items**

```python
t = (10, 20, 30, 40)
print(t[0])      # 10
print(t[-1])     # 40
print(t[1:3])    # (20, 30)
```

---

## 🚫 **Editing Items**

```python
t = (1, 2, 3)
t[0] = 99  # ❌ Error: 'tuple' object does not support item assignment
```

If you need to modify, convert it to a list:

```python
temp = list(t)
temp[0] = 99
t = tuple(temp)
```

---

## ➕ **Adding Items**

Tuples are immutable, but you can **create a new tuple** by concatenation:

```python
t = (1, 2)
t += (3, 4)
print(t)  # (1, 2, 3, 4)
```

---

## ❌ **Deleting Items**

You cannot delete individual items, but you can delete the entire tuple:

```python
del t  # Deletes the whole tuple variable
```

---

## ⚙️ **Operations on Tuples**

```python
t = (1, 2, 3)
print(t + (4, 5))      # Concatenation → (1, 2, 3, 4, 5)
print(t * 2)           # Repetition → (1, 2, 3, 1, 2, 3)
print(2 in t)          # Membership → True
```

---

## 🧰 **Tuple Functions**

|Function|Description|
|---|---|
|`len(t)`|Returns number of elements|
|`max(t)`|Returns maximum value|
|`min(t)`|Returns minimum value|
|`sum(t)`|Sums elements (only numbers)|
|`t.index(x)`|Returns first index of element `x`|
|`t.count(x)`|Counts occurrences of `x`|
|`tuple(iter)`|Converts iterable to tuple|

---

## 🔄 Tuple Packing & Unpacking

```python
t = (1, 2, 3)
a, b, c = t
print(a, b, c)  # 1 2 3
```

  **Difference Between Lists and Tuples**

| Feature                | **List**                                   | **Tuple**                                                   |
| ---------------------- | ------------------------------------------ | ----------------------------------------------------------- |
| **Syntax**             | `[]` or `list()`                           | `()` or `tuple()`                                           |
| **Mutability**         | ✅ **Mutable** — can be modified            | ❌ **Immutable** — cannot be changed                         |
| **Speed**              | Slower due to flexibility                  | Faster due to immutability                                  |
| **Memory**             | Uses **more memory**                       | Uses **less memory**                                        |
| **Built-in Functions** | Rich set (e.g., `append()`, `pop()`)       | Limited (only `count()`, `index()`)                         |
| **Error Prone**        | More prone to accidental changes           | Safer for fixed data                                        |
| **Usability**          | Best for dynamic, frequently changing data | Best for **constant data**, function returns, keys in dicts |
