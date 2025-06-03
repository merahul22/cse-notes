
## 📌 What is Time Complexity?

**Time complexity** measures how the runtime of a program grows **with respect to the input size (`n`)**. It helps us understand the **efficiency** of algorithms.

---

## 🧠 Why Time Complexity Matters

- Predict performance on large datasets
    
- Optimize slow programs
    
- Avoid inefficient loops and operations
    

---

## ⏱️ Big-O Notation (Most Common Types)

|Big-O|Name|Example|
|---|---|---|
|`O(1)`|Constant Time|Accessing element in list: `lst[0]`|
|`O(log n)`|Logarithmic Time|Binary search|
|`O(n)`|Linear Time|Iterating over a list|
|`O(n log n)`|Linearithmic|Merge sort, quick sort|
|`O(n²)`|Quadratic Time|Nested loops over list|
|`O(2ⁿ)`|Exponential Time|Recursive Fibonacci|
|`O(n!)`|Factorial Time|Generating all permutations|

---

## 🧩 Time Complexities of Common Python Operations

### 📋 Lists

|Operation|Time Complexity|
|---|---|
|Indexing `lst[i]`|O(1)|
|Append|O(1)|
|Insert/Delete|O(n)|
|Iterate|O(n)|
|Sort|O(n log n)|
|Check membership|O(n)|

### 📖 Dictionaries (Hash Tables)

|Operation|Time Complexity|
|---|---|
|Access by key `d[k]`|O(1) (avg)|
|Insert/Delete key|O(1) (avg)|
|Iterate over dict|O(n)|
|Search key in dict|O(1) (avg)|

### 🧬 Sets

|Operation|Time Complexity|
|---|---|
|Add/Delete/Search|O(1) (avg)|
|Iterate|O(n)|
|Union/Intersection|O(len(set1) + len(set2))|

### 🧵 Strings

|Operation|Time Complexity|
|---|---|
|Concatenation|O(n)|
|Length `len(s)`|O(1)|
|Index `s[i]`|O(1)|
|Membership `x in s`|O(n)|

---

## 🧮 Examples of Time Complexities

### ✅ O(1) – Constant Time

```python
def get_first(lst):
    return lst[0]
```

### ✅ O(n) – Linear Time

```python
def print_all(lst):
    for item in lst:
        print(item)
```

### ✅ O(n²) – Quadratic Time

```python
def print_pairs(lst):
    for i in lst:
        for j in lst:
            print(i, j)
```

### ✅ O(log n) – Logarithmic Time

```python
def binary_search(arr, target):
    low, high = 0, len(arr)-1
    while low <= high:
        mid = (low + high) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            low = mid + 1
        else:
            high = mid - 1
```

---

## 🔍 How to Analyze Time Complexity

1. **Identify Loops**: Each loop adds `O(n)` or more.
    
2. **Check Recursion**: How many calls and size of input each time?
    
3. **Break into parts**: Add (`+`) for sequential, Multiply (`×`) for nested.
    

---

## 🧠 Tips for Optimizing Time Complexity

- Avoid nested loops where possible
    
- Use sets/dicts for fast lookup
    
- Use built-in functions (like `sorted`) — they're optimized
    
- Prefer list comprehensions over manual loops
    

---

## 🧪 Tools to Analyze Time

- Use `time` or `timeit` module to measure actual execution time:
    

```python
import timeit
print(timeit.timeit('sum(range(1000))', number=1000))
```

---

## 🛠️ Real-World Example: Sum of Pairs

```python
# O(n²)
def find_pair_brute(nums, target):
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            if nums[i] + nums[j] == target:
                return [i, j]

# O(n)
def find_pair_optimized(nums, target):
    seen = {}
    for i, num in enumerate(nums):
        diff = target - num
        if diff in seen:
            return [seen[diff], i]
        seen[num] = i
```

---

## 🏁 Summary Table

|Data Type|Best Lookup Time|Worst Lookup Time|
|---|---|---|
|List|O(1)|O(n)|
|Dict|O(1)|O(n)|
|Set|O(1)|O(n)|
|String|O(1)|O(n)|
