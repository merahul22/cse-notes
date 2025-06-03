
## 🔹 1. What is a Function?

A **function** is a reusable block of code that performs a specific task.

```python
def greet(name):
    return f"Hello, {name}!"
```

---

## 🔸 2. Parameters vs Arguments

|Term|Meaning|
|---|---|
|**Parameter**|Variable in the function definition|
|**Argument**|Actual value passed to the function|

```python
def add(x, y):  # x, y are parameters
    return x + y

add(2, 3)       # 2, 3 are arguments
```

---

## 🔸 3. Types of Arguments

1. **Positional Arguments**
    
2. **Keyword Arguments**
    
3. **Default Arguments**
    
4. **Variable-length Arguments (`*args`, `**kwargs`)**
    

```python
def example(a, b=2, *args, **kwargs):
    pass
```

---

## 🔸 4. `*args` vs `**kwargs`

- `*args`: Tuple of extra positional arguments
    
- `**kwargs`: Dict of extra keyword arguments
    

```python
def demo(*args, **kwargs):
    print(args)     # (1, 2, 3)
    print(kwargs)   # {'x': 10, 'y': 20}

demo(1, 2, 3, x=10, y=20)
```

---

## 🔸 5. Function Execution in Memory

- Functions are stored in **stack memory**.
    
- When called, a **stack frame** is created.
    
- Arguments and local variables live inside this frame.
    
- Once complete, the frame is popped off.
    

---

## 🔸 6. Functions Without `return` Statement

- If no `return`, Python returns `None`.
    

```python
def hello():
    print("Hi")

result = hello()
print(result)  # None
```

---

## 🔸 7. Variable Scope

|Scope|Description|
|---|---|
|Local|Inside a function|
|Global|Outside all functions|
|Nonlocal|Used in nested functions|

```python
x = 10

def f():
    x = 20  # Local
    print(x)

f()
print(x)  # Global
```

---

## 🔸 8. Nested Functions

```python
def outer():
    def inner():
        print("Inside inner")
    inner()

outer()
```

---

## 🔸 9. Functions as First-Class Citizens

You can:

- Assign functions to variables
    
- Pass them as arguments
    
- Return them from other functions
    

```python
def greet(): return "Hello"
say = greet
print(say())
```

---

## 🔸 10. Deleting Functions

```python
def temp():
    print("I exist")

del temp
# temp() → NameError
```

---

## 🔸 11. Benefits of Using Functions

- Code reusability
    
- Modular and organized code
    
- Easy testing and debugging
    
- Separation of concerns
    

---

## 🔸 12. Lambda Functions

```python
add = lambda x, y: x + y
print(add(3, 5))  # 8
```

### ✅ Lambda vs Normal Functions

|Feature|Lambda|Normal Function|
|---|---|---|
|Syntax|One-liner|Multi-line|
|Name|Usually anonymous|Always named (unless nested)|
|Return|Implicit|Explicit `return`|
|Use Case|Short, throwaway functions|Complex logic|

---

## 🔸 13. Higher-Order Functions (HOF)

Functions that take/return another function.

```python
def hof(fn):
    return fn("Python")

def shout(text):
    return text.upper()

print(hof(shout))
```

---

## 🔸 14. Built-in HOFs: `map`, `filter`, `reduce`

### ✅ `map(function, iterable)`

```python
nums = [1, 2, 3]
squares = list(map(lambda x: x*x, nums))
print(squares)  # [1, 4, 9]
```

### ✅ `filter(function, iterable)`

```python
evens = list(filter(lambda x: x % 2 == 0, nums))
print(evens)  # [2]
```

### ✅ `reduce(function, iterable)` (from `functools`)

```python
from functools import reduce
total = reduce(lambda x, y: x + y, nums)
print(total)  # 6
```

---

## 🧠 Additional Tips to Include

|Concept|Description|
|---|---|
|`return` vs `print`|`return` gives back result; `print` just displays|
|`docstrings`|Use `""" """` under function to describe its purpose|
|Recursive functions|Function that calls itself|
|Anonymous function|Another name for lambda|
|Decorators|HOFs that modify behavior of another function|


## 📌 What is a **Docstring**?

A **docstring** is a special **string literal** that is used to **document a function, class, module, or method**. It is placed **right after the definition line** and is enclosed in **triple quotes (`"""` or `'''`)**.

---

## ✅ Why Use Docstrings?

|Reason|Explanation|
|---|---|
|📚 Documentation|Explains what the function/class/module does|
|📦 IDE Support|IDEs and tools show the docstring as a tooltip|
|🧪 Introspection|Used by `help()` and `.__doc__` in Python shell|
|📁 Readability|Helps others (or future-you) understand the code|

---

## 🧠 Syntax

```python
def function_name(parameters):
    """Summary line.
    
    Optional extended description.

    Parameters:
    param1 (type): Description
    param2 (type): Description

    Returns:
    type: Description of return value
    """
    # code here
```

---

## 🔍 Example

```python
def add(a, b):
    """
    Adds two numbers.

    Parameters:
    a (int): First number
    b (int): Second number

    Returns:
    int: Sum of a and b
    """
    return a + b

print(add.__doc__)
```

**Output:**

```
Adds two numbers.

Parameters:
a (int): First number
b (int): Second number

Returns:
int: Sum of a and b
```

---

## 🛠️ Best Practices

- Use triple quotes (`"""`) even for one-line docstrings.
    
- Start with a summary in the first line.
    
- Use full sentences and proper punctuation.
    
- Follow consistent formatting (Google, NumPy, or reStructuredText style).

