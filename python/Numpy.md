

## 🔰 1. What is NumPy?

**Theory:**  
NumPy stands for **Numerical Python**. It is a fundamental package for scientific computing in Python. It provides:

- A high-performance **multi-dimensional array object** called `ndarray`.
    
- Functions for performing mathematical operations efficiently.
    
- Tools for integrating C/C++ and Fortran code.
    

**Why NumPy over Python Lists?**

- Lists are slow and memory-inefficient for numerical computations.
    
- NumPy arrays are faster and support vectorized operations (no loops needed).
    

---

## 🔢 2. Creating NumPy Arrays

### a. From a Python List – `np.array()`

**Theory:**  
`np.array()` converts a list (or list of lists) into a NumPy array.

**Example:**

```python
import numpy as np
a = np.array([1, 2, 3])
print(a)
```

**Output:**

```
[1 2 3]
```

### b. Creating 2D Array

```python
b = np.array([[1, 2], [3, 4]])
print(b)
```

**Output:**

```
[[1 2]
 [3 4]]
```

---

## 🧰 3. Special Array Creation

### a. `np.zeros(shape)`

**Theory:**  
Creates an array filled with zeros.

**Example:**

```python
np.zeros((2, 3))
```

**Output:**

```
[[0. 0. 0.]
 [0. 0. 0.]]
```

### b. `np.ones(shape)`

**Theory:**  
Creates an array filled with ones.

**Example:**

```python
np.ones((2, 2))
```

**Output:**

```
[[1. 1.]
 [1. 1.]]
```

### c. `np.eye(n)`

**Theory:**  
Creates an **identity matrix** (diagonal = 1, rest = 0).

**Example:**

```python
np.eye(3)
```

**Output:**

```
[[1. 0. 0.]
 [0. 1. 0.]
 [0. 0. 1.]]
```

### d. `np.arange(start, stop, step)`

**Theory:**  
Generates evenly spaced values within a range.

**Example:**

```python
np.arange(1, 10, 2)
```

**Output:**

```
[1 3 5 7 9]
```

### e. `np.linspace(start, stop, num)`

**Theory:**  
Generates `num` evenly spaced values from start to stop (inclusive).

**Example:**

```python
np.linspace(0, 1, 5)
```

**Output:**

```
[0.   0.25 0.5  0.75 1.  ]
```

---

## 🧾 4. Array Attributes

**Theory:**  
You can access properties of an array like shape, size, dimensions, etc.

```python
arr = np.array([[1, 2], [3, 4], [5, 6]])
print(arr.shape)   # (3, 2) -> 3 rows, 2 columns
print(arr.size)    # 6 elements
print(arr.ndim)    # 2D
print(arr.dtype)   # int64
```

---

## 🔄 5. Array Reshaping

**Theory:**  
You can **reshape** the array to a different shape using `.reshape()`.

```python
a = np.arange(6)       # [0 1 2 3 4 5]
b = a.reshape((2, 3))  # 2 rows, 3 cols
print(b)
```

**Output:**

```
[[0 1 2]
 [3 4 5]]
```

---

## ➕ 6. Mathematical Operations

### a. Element-wise Operations

```python
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])
print(a + b)
print(a * b)
print(a ** 2)
```

**Output:**

```
[5 7 9]
[ 4 10 18]
[1 4 9]
```

### b. Aggregate Functions

```python
a = np.array([1, 2, 3, 4])
print(np.sum(a))       # 10
print(np.mean(a))      # 2.5
print(np.min(a))       # 1
print(np.max(a))       # 4
print(np.std(a))       # 1.118
```

---

## 🧱 7. Stacking Arrays

### a. `np.vstack` – Vertical Stack

```python
a = np.array([1, 2])
b = np.array([3, 4])
print(np.vstack((a, b)))
```

**Output:**

```
[[1 2]
 [3 4]]
```

### b. `np.hstack` – Horizontal Stack

```python
print(np.hstack((a, b)))
```

**Output:**

```
[1 2 3 4]
```

---

## ✂️ 8. Splitting Arrays

### a. `np.hsplit` – Horizontal Split

```python
a = np.array([[1, 2, 3], [4, 5, 6]])
print(np.hsplit(a, 3))  # Split into 3 cols
```

**Output:**

```
[array([[1],
        [4]]), array([[2],
                      [5]]), array([[3],
                                    [6]])]
```

---

## ✅ Summary

|Operation|Function|Example|
|---|---|---|
|Create array|`np.array()`|1D, 2D arrays|
|Zeros/Ones|`np.zeros`, `np.ones`|All elements 0 or 1|
|Ranges|`np.arange`, `np.linspace`|Sequence of numbers|
|Shape & Type|`arr.shape`, `arr.dtype`|Get metadata|
|Reshape|`arr.reshape()`|Change dimensions|
|Math Operations|`+`, `*`, `np.sum()` etc.|Vectorized math|
|Stack/Split|`np.hstack`, `np.hsplit`|Combine or divide|


## 📌 NumPy Arrays vs Python Lists

### 1. **Speed Comparison**

#### 💡 Theory:

- Python lists are slower because they are general-purpose containers.
    
- NumPy arrays are implemented in C and use contiguous memory blocks and vectorized operations.
    

#### 🔢 Code:

```python
import time

# Python List
a = [i for i in range(10000000)]
b = [i for i in range(10000000, 20000000)]
start = time.time()
c = [a[i] + b[i] for i in range(len(a))]
print("List time:", time.time() - start)

# NumPy Array
import numpy as np
a = np.arange(10000000)
b = np.arange(10000000, 20000000)
start = time.time()
c = a + b
print("NumPy time:", time.time() - start)
```

#### ✅ Output:

- List time: ~3 sec
    
- NumPy time: ~0.04 sec
    

---

### 2. **Memory Efficiency**

#### 💡 Theory:

- Lists store object references, taking more memory.
    
- NumPy arrays store data in compact form.
    

#### 🔢 Code:

```python
import sys

# Python list memory
a = [i for i in range(10000000)]
print("List size:", sys.getsizeof(a))

# NumPy array memory
a = np.arange(10000000, dtype=np.int8)
print("NumPy size:", sys.getsizeof(a))
```

---

## 📌 Indexing and Slicing

### 1. **Normal Indexing**

```python
a = np.arange(24).reshape(6,4)
print(a[1,2])         # Single element
print(a[1:3,1:3])     # Slicing
```

### 2. **Fancy Indexing**

```python
print(a[:, [0, 2, 3]])  # Select specific columns
```

### 3. **Boolean Indexing**

```python
a = np.random.randint(1,100,24).reshape(6,4)
print(a[a > 50])               # Numbers > 50
print(a[a % 2 == 0])           # Even numbers
print(a[(a > 50) & (a % 2 == 0)])  # Both conditions
print(a[~(a % 7 == 0)])        # Not divisible by 7
```

---

## 📌 Broadcasting

### 💡 Theory:

Broadcasting allows operations on arrays of different shapes.  
Rules:

1. If shapes are different, prepend `1`s to the smaller shape.
    
2. Dimensions are compatible when they are equal or one is `1`.
    
3. Else, error is thrown.
    

### ✅ Examples:

```python
a = np.arange(6).reshape(2, 3)
b = np.arange(3).reshape(1, 3)
print(a + b)
```

```python
a = np.arange(3).reshape(1,3)
b = np.arange(4).reshape(4,1)
print(a + b)
```

```python
# Invalid shape: will give error
a = np.arange(12).reshape(3,4)
b = np.arange(12).reshape(4,3)
print(a + b)  # Incompatible shapes
```

---

## 📌 Mathematical Operations

### 1. **Trigonometric**

```python
a = np.arange(10)
print(np.sin(a))
```

### 2. **Sigmoid Function**

```python
def sigmoid(x):
    return 1 / (1 + np.exp(-x))

a = np.arange(100)
print(sigmoid(a))
```

---

## 📌 Error Metrics

### 1. **Mean Squared Error**

```python
actual = np.random.randint(1, 50, 25)
predicted = np.random.randint(1, 50, 25)

def mse(actual, predicted):
    return np.mean((actual - predicted)**2)

print(mse(actual, predicted))
```

---

## 📌 Handling Missing Values

### 💡 Theory:

`np.nan` is used to represent missing values. Use `np.isnan()` to filter them.

```python
a = np.array([1, 2, 3, 4, np.nan, 6])
print(a[~np.isnan(a)])  # Removes nan
```

---

## 📌 Plotting Graphs with Matplotlib

### 1. **y = x**

```python
import matplotlib.pyplot as plt
x = np.linspace(-10,10,100)
y = x
plt.plot(x, y)
plt.title("y = x")
plt.show()
```

### 2. **y = x²**

```python
y = x**2
plt.plot(x, y)
plt.title("y = x²")
plt.show()
```

### 3. **y = sin(x)**

```python
y = np.sin(x)
plt.plot(x, y)
plt.title("y = sin(x)")
plt.show()
```

### 4. **y = x log(x)** (excluding x ≤ 0 to avoid log error)

```python
x = np.linspace(0.1, 10, 100)
y = x * np.log(x)
plt.plot(x, y)
plt.title("y = x log(x)")
plt.show()
```

### 5. **Sigmoid Plot**

```python
x = np.linspace(-10,10,100)
y = 1 / (1 + np.exp(-x))
plt.plot(x, y)
plt.title("Sigmoid Function")
plt.show()
```


### ✅ `np.sort()`

**Purpose**: Returns a sorted copy of an array.

**Syntax**:

```python
np.sort(array, axis=-1)
```

- `axis=-1`: Default. Sorts along the last axis.
    
- `axis=0`: Sorts each column (downward).
    
- `axis=1`: Sorts each row (horizontally).
    

**Example**:

```python
a = np.array([5, 2, 9, 1])
np.sort(a)             # Output: array([1, 2, 5, 9])
```

---

### ✅ `np.append()`

**Purpose**: Adds values to the end of an array.

**Syntax**:

```python
np.append(arr, values, axis=None)
```

- If `axis=None`, it flattens both arrays.
    

**Example**:

```python
a = np.array([1, 2, 3])
np.append(a, [4, 5])   # Output: array([1, 2, 3, 4, 5])
```

---

### ✅ `np.concatenate()`

**Purpose**: Joins two or more arrays along an axis.

**Syntax**:

```python
np.concatenate((arr1, arr2), axis=0)
```

**Example**:

```python
a = np.array([[1, 2], [3, 4]])
b = np.array([[5, 6]])
np.concatenate((a, b), axis=0)
# Output: array([[1, 2], [3, 4], [5, 6]])
```

---

### ✅ `np.unique()`

**Purpose**: Returns the sorted unique elements of an array.

**Syntax**:

```python
np.unique(arr)
```

**Example**:

```python
a = np.array([1, 2, 2, 3])
np.unique(a)           # Output: array([1, 2, 3])
```

---

### ✅ `np.expand_dims()`

**Purpose**: Expands the shape of an array by inserting a new axis.

**Syntax**:

```python
np.expand_dims(arr, axis)
```

**Example**:

```python
a = np.array([1, 2, 3])
np.expand_dims(a, axis=0).shape  # Output: (1, 3)
```

---

### ✅ `np.where()`

**Purpose**: Returns indices where a condition is true or selects values based on a condition.

**Syntax**:

```python
np.where(condition, x, y)
```

**Example**:

```python
a = np.array([10, 20, 30])
np.where(a > 15, 1, 0)  # Output: array([0, 1, 1])
```

---

### ✅ `np.argmax()` / `np.argmin()`

**Purpose**: Returns the index of the maximum (or minimum) value.

**Syntax**:

```python
np.argmax(arr, axis=None)
```

**Example**:

```python
a = np.array([10, 20, 5])
np.argmax(a)           # Output: 1
```

---

### ✅ `np.cumsum()` / `np.cumprod()`

**Purpose**:

- `cumsum`: Cumulative sum.
    
- `cumprod`: Cumulative product.
    

**Syntax**:

```python
np.cumsum(arr, axis=None)
```

**Example**:

```python
a = np.array([1, 2, 3])
np.cumsum(a)           # Output: array([1, 3, 6])
```

---

### ✅ `np.percentile()` / `np.median()`

**Purpose**:

- `percentile`: N-th percentile value.
    
- `median`: Middle value.
    

**Syntax**:

```python
np.percentile(arr, q)
```

**Example**:

```python
a = np.array([1, 3, 2])
np.percentile(a, 50)   # Output: 2.0
```

---

### ✅ `np.histogram()`

**Purpose**: Computes histogram (frequency distribution) of the data.

**Syntax**:

```python
np.histogram(arr, bins)
```

**Example**:

```python
a = np.array([1, 2, 1])
np.histogram(a, bins=[0,1,2,3])  # Output: (array([0, 2, 1]), ...)
```

---

### ✅ `np.corrcoef()`

**Purpose**: Returns Pearson correlation coefficient matrix.

**Syntax**:

```python
np.corrcoef(arr1, arr2)
```

**Example**:

```python
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])
np.corrcoef(a, b)
```

---

### ✅ `np.isin()`

**Purpose**: Tests whether each element in one array is in another.

**Syntax**:

```python
np.isin(arr, test_elements)
```

**Example**:

```python
a = np.array([1, 2, 3])
np.isin(a, [2, 4])     # Output: array([False, True, False])
```

---

### ✅ `np.flip()`

**Purpose**: Reverses array elements.

**Syntax**:

```python
np.flip(arr, axis=None)
```

**Example**:

```python
a = np.array([1, 2, 3])
np.flip(a)             # Output: array([3, 2, 1])
```

---

### ✅ `np.put()`

**Purpose**: Replaces elements at specified indices.

**Syntax**:

```python
np.put(arr, indices, values)
```

**Example**:

```python
a = np.array([1, 2, 3])
np.put(a, [0, 2], [10, 30])
```

---

### ✅ `np.delete()`

**Purpose**: Removes sub-arrays along the given axis.

**Syntax**:

```python
np.delete(arr, obj, axis=None)
```

**Example**:

```python
a = np.array([1, 2, 3])
np.delete(a, 1)        # Output: array([1, 3])
```

---

### ✅ Set Functions

```python
np.union1d(m, n)       # All unique elements
np.intersect1d(m, n)   # Common elements
np.setdiff1d(n, m)     # Elements in n but not in m
np.setxor1d(m, n)      # Symmetric difference
np.in1d(m, [2])        # Element-wise check
```

---

### ✅ `np.clip()`

**Purpose**: Limits the values in an array between `a_min` and `a_max`.

**Syntax**:

```python
np.clip(arr, a_min, a_max)
```

**Example**:

```python
a = np.array([1, 10, 100])
np.clip(a, 5, 50)      # Output: array([ 5, 10, 50])
```


### ✅ `np.swapaxes()`

**Purpose**:  
Swaps two axes (dimensions) of an array.

**Syntax**:

```python
np.swapaxes(array, axis1, axis2)
```

**Use Case**: Useful for changing orientation of data like from row-wise to column-wise.

**Example**:

```python
a = np.array([[1, 2, 3], [4, 5, 6]])
a.shape                 # (2, 3)
np.swapaxes(a, 0, 1)    # Output: array([[1, 4], [2, 5], [3, 6]])
```

---

### ✅ `np.count_nonzero()`

**Purpose**:  
Counts the number of non-zero elements in the array.

**Syntax**:

```python
np.count_nonzero(array, axis=None)
```

**Example**:

```python
a = np.array([[0, 1, 2], [3, 0, 4]])
np.count_nonzero(a)           # Output: 4
np.count_nonzero(a, axis=0)   # Output: array([1, 1, 2])
```

---

### ✅ `np.tile()`

**Purpose**:  
Repeats the array in the specified pattern.

**Syntax**:

```python
np.tile(array, reps)
```

- `reps` can be a single int or a tuple to repeat in multiple dimensions.
    

**Example**:

```python
a = np.array([1, 2])
np.tile(a, 3)          # Output: array([1, 2, 1, 2, 1, 2])
np.tile(a, (2, 2))     # Output: array([[1, 2, 1, 2], [1, 2, 1, 2]])
```

---

### ✅ `np.repeat()`

**Purpose**:  
Repeats elements of an array.

**Syntax**:

```python
np.repeat(array, repeats, axis=None)
```

**Example**:

```python
a = np.array([1, 2, 3])
np.repeat(a, 2)         # Output: array([1, 1, 2, 2, 3, 3])
```

With axis:

```python
a = np.array([[1, 2], [3, 4]])
np.repeat(a, 2, axis=0)
# Output: array([[1, 2], [1, 2], [3, 4], [3, 4]])
```

---

### ✅ `np.allclose()`

**Purpose**:  
Checks whether two arrays are element-wise equal within a tolerance (used in floating-point comparisons).

**Syntax**:

```python
np.allclose(arr1, arr2, rtol=1e-5, atol=1e-8)
```

- `rtol`: relative tolerance
    
- `atol`: absolute tolerance
    

**Example**:

```python
a = np.array([1.000001, 2.000001])
b = np.array([1.000002, 2.000002])
np.allclose(a, b)       # Output: True
```

---

### ✨ Summary Table for Quick Revision:

|Function|Purpose|Key Feature|
|---|---|---|
|`swapaxes()`|Swap 2 axes|Rearranging matrix dimensions|
|`count_nonzero()`|Count non-zero elements|Good for filtering|
|`tile()`|Repeat array in blocks|Tiling layout|
|`repeat()`|Repeat each element|Useful in broadcasting-like operations|
|`allclose()`|Compare arrays with tolerance|Floating-point precision checks|
