

# 🧠 Python Namespaces, Scope, and Decorator

---

## 🔷 1. What is a Namespace?

A **namespace** is a system to ensure that all the names (variables, functions, objects) in a program are unique and can be used without conflict.

🧾 **Think of it as a dictionary**:  
Each name you define (like a variable `x`) is a key in this dictionary, and the object it refers to (like `5`) is the value.

---

### ✅ Types of Namespaces

|Namespace Type|Description|
|---|---|
|**Built-in**|Includes names like `print()`, `len()`, etc., created when Python starts|
|**Global**|Names declared at the top-level of a script or module|
|**Enclosing**|Names in the local scope of enclosing functions (in nested functions)|
|**Local**|Names inside a function or block|

---

## 🔶 2. What is Scope?

A **scope** is the area of a program where a namespace is directly accessible.

Python uses the **LEGB Rule** to search for variables:

**L → Local →** Inside the current function  
**E → Enclosing →** In the enclosing function (if nested)  
**G → Global →** In the main script/module  
**B → Built-in →** Python’s built-in functions and constants

---

### 🧪 Example: Local and Global Variables

```python
a = 10  # Global

def my_func():
    b = 5  # Local
    print("Inside function:", b)

my_func()
print("Outside function:", a)
```

**Output:**

```
Inside function: 5
Outside function: 10
```

---

### ⚠️ When Local and Global Variables Have the Same Name

```python
a = 10

def my_func():
    a = 20  # This is local, not global
    print("Inside function:", a)

my_func()
print("Outside function:", a)
```

**Output:**

```
Inside function: 20
Outside function: 10
```

---

### ❌ Editing Global Variable Without `global` Keyword

```python
a = 10

def my_func():
    a += 1     # ❌ UnboundLocalError
    print(a)

my_func()
```

**Output:**

```
UnboundLocalError: local variable 'a' referenced before assignment
```

---

### ✅ Using `global` to Modify Global Variable

```python
a = 10

def my_func():
    global a
    a += 1
    print("Inside:", a)

my_func()
print("Outside:", a)
```

**Output:**

```
Inside: 11
Outside: 11
```

---

### ✅ Creating Global Variable Inside Function

```python
def my_func():
    global a
    a = 100

my_func()
print("Global a:", a)
```

**Output:**

```
Global a: 100
```

---

### ✅ Function Parameter is Always Local

```python
def show(x):
    print("Inside function:", x)

x = 50
show(100)
print("Outside function:", x)
```

**Output:**

```
Inside function: 100
Outside function: 50
```

---

## 🔷 3. Built-in Namespace

These are functions like `max`, `len`, `print` etc.

### 🧪 See All Built-ins

```python
import builtins
print(dir(builtins))
```

---

### ⚠️ Overwriting a Built-in Function

```python
L = [1, 2, 3]
print(max(L))  # 3

def max():
    print("Overwritten!")

max()  # Works as our function
# print(max(L))  # ❌ TypeError
```

---

## 🔶 4. Enclosing Scope (Nested Functions)

```python
def outer():
    a = 5
    def inner():
        print("Inner:", a)
    inner()

outer()
```

**Output:**

```
Inner: 5
```

---

### ✅ Using `nonlocal` to Modify Enclosing Variable

```python
def outer():
    a = 5
    def inner():
        nonlocal a
        a += 1
        print("Inner:", a)
    inner()
    print("Outer:", a)

outer()
```

**Output:**

```
Inner: 6
Outer: 6
```

---

## 🧠 Summary of Namespace and Scope

|Term|Meaning|
|---|---|
|Namespace|A container that holds variable names and their values|
|Scope|The region where you can access a variable|
|LEGB Rule|Order of lookup: Local → Enclosing → Global → Built-in|
|`global`|Used to modify global variables from inside a function|
|`nonlocal`|Used in nested functions to modify variables from the enclosing scope|

---

## 🌟 5. Python Decorators

### 🧾 What is a Decorator?

A **decorator** is a function that takes another function as input, adds extra functionality, and returns the new function.

✅ Functions are **first-class objects** in Python, so they can be passed around like variables.

---

### 🎯 Simple Decorator Example

```python
def my_decorator(func):
    def wrapper():
        print("Before")
        func()
        print("After")
    return wrapper

@my_decorator
def greet():
    print("Hello!")

greet()
```

**Output:**

```
Before
Hello!
After
```

---

## 🎯 Decorators with Arguments Using `*args`

```python
def timer(func):
    def wrapper(*args):
        import time
        start = time.time()
        func(*args)
        end = time.time()
        print("Time taken:", end - start)
    return wrapper

@timer
def slow_func(n):
    import time
    time.sleep(n)
    print(f"Slept for {n} seconds")

slow_func(2)
```

**Output:**

```
Slept for 2 seconds
Time taken: 2.00023...
```

✅ `*args` allows the decorator to work with functions that take arguments.

---

## 🎯 Decorator with Parameters (Decorator Factory)

```python
def check_type(datatype):
    def outer(func):
        def wrapper(*args):
            if isinstance(args[0], datatype):
                func(*args)
            else:
                raise TypeError("Wrong data type!")
        return wrapper
    return outer

@check_type(int)
def square(n):
    print(n ** 2)

square(5)     # Works
square("hi")  # ❌ TypeError
```

---

## ✅ Built-in Decorators

|Decorator|Purpose|
|---|---|
|`@staticmethod`|Declares a static method inside a class|
|`@classmethod`|Method that takes class as first argument|
|`@property`|Converts a method into a property|
|`@abstractmethod`|Used with abstract base classes|

---

## 🧾 Conclusion and Best Practices

- Always understand **LEGB rule** to debug variable access.
    
- Use `global` or `nonlocal` only when needed.
    
- Decorators are powerful – use them for **logging**, **timing**, **authorization**, etc.
    
- Use `*args` and `**kwargs` in decorators to support any function signature.
