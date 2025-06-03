

## ✅ 1. **What are Lists?**

A **list** in Python is an **ordered, mutable** collection of items that can contain **elements of different types** (integers, strings, other lists, etc.).

```python
my_list = [1, "hello", 3.14, [2, 4]]
```

## 🔁 2. **Lists vs Arrays**

|Feature|List|Array (`array` module)|
|---|---|---|
|Data types|Can hold mixed data types|Holds same data type only|
|Flexibility|More flexible (heterogeneous)|Less flexible|
|Speed|Slower for numeric operations|Faster for numeric computations|
|Use case|General-purpose|Numerical data processing|
|Import needed?|No|Yes (`from array import array`)|


## 📌 3. **Characteristics of a List**

- **Ordered**: Items have a defined order.
    
- **Mutable**: You can change, add, or remove elements.
    
- **Indexed**: Items can be accessed via index.
    
- **Allows duplicates**: Items can be repeated.
    
- **Can be nested**: Lists can contain other lists.
    

## 🛠️ 4. **How to Create a List**

```python
empty = []
nums = [1, 2, 3]
mixed = [1, "apple", 3.14]
nested = [1, [2, 3], 4]
```


## 🔍 5. **Access Items from a List**

```python
a = [10, 20, 30, 40]
print(a[0])    # 10
print(a[-1])   # 40
print(a[1:3])  # [20, 30]
```

## ✏️ 6. **Editing Items in a List**

```python
a = [10, 20, 30]
a[1] = 99
print(a)  # [10, 99, 30]
```



## ❌ 7. **Deleting Items from a List**

```python
a = [10, 20, 30]
del a[1]        # Removes by index
a.remove(30)    # Removes by value
a.pop()         # Removes last item
a.clear()       # Empties the list
```


## ⚙️ 8. **Operations on Lists**

```python
a = [1, 2]
b = [3, 4]

print(a + b)         # [1, 2, 3, 4]
print(a * 2)         # [1, 2, 1, 2]
print(2 in a)        # True
print(len(a))        # 2
```


## 🧰 9. **Functions on Lists**

|Function|Description|
|---|---|
|`append(x)`|Adds `x` to the end of the list|
|`insert(i, x)`|Inserts `x` at index `i`|
|`extend(lst)`|Adds elements from another iterable|
|`remove(x)`|Removes first occurrence of `x`|
|`pop([i])`|Removes and returns item at index `i` (default last)|
|`clear()`|Empties the list|
|`sort()`|Sorts list in place|
|`reverse()`|Reverses list in place|
|`index(x)`|Returns index of first occurrence of `x`|
|`count(x)`|Returns the count of `x`|
|`copy()`|Returns a shallow copy|


## 📘 **List Comprehension in Python**

List comprehension provides a **concise** and **elegant** way to create lists using a single line of code.

---

### 🧠 **Syntax:**

```python
new_list = [expression for item in iterable if condition]
```

- **expression**: The value to store in the list.
    
- **item**: The variable representing each element in the iterable.
    
- **iterable**: A sequence (like list, tuple, range, etc.)
    
- **condition** _(optional)_: A filter that decides whether to include the item.


### ✅ **Basic Example:**

```python
# Create a list of squares from 0 to 9
squares = [x*x for x in range(10)]
print(squares)  # [0, 1, 4, 9, ..., 81]
```

### ✅ **With Condition:**

```python
# Only even squares
even_squares = [x*x for x in range(10) if x % 2 == 0]
print(even_squares)  # [0, 4, 16, 36, 64]
```


### 💡 **Nested Loops in List Comprehension:**

```python
pairs = [(x, y) for x in [1, 2] for y in [3, 4]]
print(pairs)  # [(1, 3), (1, 4), (2, 3), (2, 4)]
```

## 🚀 **Advantages of List Comprehension**

|Feature|Benefit|
|---|---|
|✅ **Concise**|One line replaces multiple lines of loops|
|✅ **Faster**|Often more time- and space-efficient|
|✅ **Readable**|Easier to understand for simple tasks|
|✅ **Functional style**|Cleaner transformation of data|


### 🆚 **Loop vs List Comprehension Example**

#### 🔁 Loop:

```python
result = []
for i in range(5):
    result.append(i * 2)
```

#### ✅ List Comprehension:

```python
result = [i * 2 for i in range(5)]
```


## 🔗 `zip()` Function in Python

`zip()` is used to **combine multiple iterables (like lists, tuples)** into a single iterator of tuples, where each tuple contains elements from the input iterables at the same position.

### 🧰 **Syntax:**

```python
zip(iterable1, iterable2, ..., iterableN)
```

- Takes any number of iterables.
    
- Returns an iterator of tuples.
    
- Stops when the shortest iterable is exhausted.

### ✅ **Basic Example:**

```python
names = ['Alice', 'Bob', 'Charlie']
scores = [85, 90, 95]

zipped = zip(names, scores)
print(list(zipped))  
# Output: [('Alice', 85), ('Bob', 90), ('Charlie', 95)]
```

### 🔄 **Unzipping using `*`:**

```python
zipped = [('Alice', 85), ('Bob', 90)]
names, scores = zip(*zipped)

print(names)   # ('Alice', 'Bob')
print(scores)  # (85, 90)
```


### ⚠️ **Different Length Iterables:**

If iterables have different lengths, `zip()` stops at the shortest:

```python
a = [1, 2, 3]
b = ['a', 'b']

print(list(zip(a, b)))  
# Output: [(1, 'a'), (2, 'b')]
```


### 🧑‍💻 **Using `zip()` in loops:**

```python
for name, score in zip(names, scores):
    print(f"{name} scored {score}")
```


### 💡 **Common Use Cases:**

- Pairing elements from multiple lists.
    
- Creating dictionaries from two lists:


```python
keys = ['name', 'age', 'city']
values = ['Alice', 25, 'NY']
my_dict = dict(zip(keys, values))
print(my_dict)
# Output: {'name': 'Alice', 'age': 25, 'city': 'NY'}
```
