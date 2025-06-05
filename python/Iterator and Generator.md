

## 🔁 **ITERATION**

**Iteration** refers to the process of accessing elements one by one from a collection or sequence (like list, tuple, etc.).

### 🧠 Concept:

Any time we use a loop (like `for` or `while`) to go over items, it's iteration.

```python
nums = [1, 2, 3]
for i in nums:
    print(i)
```

**Output:**

```
1
2
3
```

---

## 🔄 **ITERATOR**

An **iterator** is an object that enables traversing through a collection **one element at a time**.

### ✅ Must Implement:

- `__iter__()`: Returns the iterator object itself.
    
- `__next__()`: Returns the next item, raises `StopIteration` when done.
    

### 🔍 Example:

```python
nums = [1, 2, 3]
it = iter(nums)  # creates an iterator object

print(next(it))  # 1
print(next(it))  # 2
print(next(it))  # 3
print(next(it))  # raises StopIteration
```

---

## 🧵 **ITERABLE vs ITERATOR**

|Feature|Iterable|Iterator|
|---|---|---|
|Purpose|Can return an iterator|Can be iterated using `next()`|
|Has `__iter__()`|✅|✅|
|Has `__next__()`|❌|✅|
|Examples|list, tuple, dict, set, str|iter(obj), file handlers|

```python
L = [1, 2, 3]
print(hasattr(L, '__iter__'))       # True
print(hasattr(L, '__next__'))       # False

it = iter(L)
print(hasattr(it, '__iter__'))      # True
print(hasattr(it, '__next__'))      # True
```

---

## 🔍 Internals of `for` Loop:

```python
nums = [1, 2, 3]
it = iter(nums)

while True:
    try:
        item = next(it)
        print(item)
    except StopIteration:
        break
```

---

## 🛠 Custom Iterator Example:

```python
class MyRange:
    def __init__(self, start, end):
        self.start = start
        self.end = end

    def __iter__(self):
        return MyRangeIterator(self)

class MyRangeIterator:
    def __init__(self, obj):
        self.obj = obj

    def __iter__(self):
        return self

    def __next__(self):
        if self.obj.start >= self.obj.end:
            raise StopIteration
        current = self.obj.start
        self.obj.start += 1
        return current

x = MyRange(1, 5)
for i in x:
    print(i)
```

**Output:**

```
1
2
3
4
```

---

## ⚡ WHY USE ITERATOR?

- Memory-efficient
    
- Great for large or infinite sequences
    
- Lazy evaluation (values are calculated only when needed)
    

---

## 🌱 **GENERATOR**

A **generator** is a simpler way to create an iterator using `yield` instead of class-based methods.

### ✅ Features:

- Automatically implements both `__iter__()` and `__next__()`.
    
- Maintains **state** between `yield` calls.
    
- Used when values are produced on the fly.
    

### 🔍 Simple Example:

```python
def gen_demo():
    yield "first"
    yield "second"
    yield "third"

g = gen_demo()
for i in g:
    print(i)
```

**Output:**

```
first
second
third
```

---

## ⚙️ Generator vs Function

|Function|Generator|
|---|---|
|Uses `return`|Uses `yield`|
|Terminates immediately|Can be resumed|
|Returns value|Returns iterator|

---

## 🧮 Generator for Squares

```python
def squares(n):
    for i in range(1, n+1):
        yield i**2

g = squares(5)
print(next(g))  # 1
print(next(g))  # 4
print(next(g))  # 9
```

**Continues:**

```python
for i in g:
    print(i)  # 16, 25
```

---

## 🧪 Generator Expression (like list comprehension)

```python
L = [i**2 for i in range(1, 6)]
G = (i**2 for i in range(1, 6))

print(L)            # [1, 4, 9, 16, 25]
print(next(G))      # 1
print(next(G))      # 4
```

---

## 📂 Real-life Example (Image Generator)

```python
import os, cv2

def image_loader(folder_path):
    for file in os.listdir(folder_path):
        yield cv2.imread(os.path.join(folder_path, file))
```

---

## 💡 BENEFITS OF GENERATORS

### ✅ 1. Simpler Code

Replaces complex iterator classes.

### ✅ 2. Memory Efficient

```python
L = [x for x in range(100000)]
G = (x for x in range(100000))

import sys
print(sys.getsizeof(L))  # ~800000
print(sys.getsizeof(G))  # ~100
```

### ✅ 3. Infinite Sequences

```python
def all_even():
    n = 0
    while True:
        yield n
        n += 2

evens = all_even()
print(next(evens))  # 0
print(next(evens))  # 2
```

### ✅ 4. Chaining Generators

```python
def fib(n):
    a, b = 0, 1
    for _ in range(n):
        a, b = b, a + b
        yield a

def square(nums):
    for i in nums:
        yield i**2

print(sum(square(fib(10))))  # 4895
```

---

## 🧠 Summary Table

|Feature|Iterator|Generator|
|---|---|---|
|Definition|Object for iteration|Special kind of iterator|
|Created using|Class with `__iter__`, `__next__`|`def` with `yield`|
|Memory efficient?|Yes|Yes|
|Infinite support|Yes|Yes|
|Reusability|No|No|
