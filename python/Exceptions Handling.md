# 🧠 Python Exception Handling 

## 🔹 1. Difference between **Syntax Errors** and **Exceptions**

|Syntax Error|Exception|
|---|---|
|Occurs **before execution**|Occurs **during execution**|
|Related to incorrect code syntax|Related to unexpected behavior|
|Caught by the **interpreter**|Caught by the **program**|

---

## 🔹 2. Examples of **Common Syntax Errors**

### 🔸 Missing colon:

```python
a = 5
if a == 5
    print("Hello")
```

❌ Output: `SyntaxError: expected ':'`

### 🔸 Incorrect indentation:

```python
a = 5
if a == 5:
print("Hello")
```

❌ Output: `IndentationError: expected an indented block`

### 🔸 Misspelled keyword:

```python
a = 5
iff a == 5:
    print("Hello")
```

❌ Output: `SyntaxError: invalid syntax`

### 🔸 Empty block:

```python
if True:
```

❌ Output: `IndentationError: expected an indented block`

---

## 🔹 3. Examples of **Built-in Exceptions**

### 🔸 IndexError

```python
L = [1, 2, 3]
print(L[100])
```

❌ Output: `IndexError: list index out of range`

### 🔸 ModuleNotFoundError

```python
import mathi
```

❌ Output: `ModuleNotFoundError: No module named 'mathi'`

### 🔸 KeyError

```python
d = {'name': 'Rahul'}
print(d['age'])
```

❌ Output: `KeyError: 'age'`

### 🔸 TypeError

```python
print(1 + 'a')
```

❌ Output: `TypeError: unsupported operand type(s) for +: 'int' and 'str'`

### 🔸 ValueError

```python
int('a')
```

❌ Output: `ValueError: invalid literal for int() with base 10: 'a'`

### 🔸 NameError

```python
print(x)
```

❌ Output: `NameError: name 'x' is not defined`

### 🔸 AttributeError

```python
L = [1, 2, 3]
L.upper()
```

❌ Output: `AttributeError: 'list' object has no attribute 'upper'`

---

## 🔹 4. Why Exception Handling is Important

- Prevent program from crashing
    
- Handle errors gracefully
    
- Improve user experience
    

---

## 🔹 5. Try-Except Block

```python
try:
    with open('sample1.txt', 'r') as f:
        print(f.read())
except:
    print("Sorry, file not found")
```

✅ Output: `Sorry, file not found`

---

## 🔹 6. Catching Specific Exceptions

```python
try:
    f = open('sample1.txt', 'r')
    print(f.read())
    print(5 / 0)
except FileNotFoundError:
    print("File not found")
except ZeroDivisionError:
    print("Can't divide by 0")
except Exception as e:
    print(e)
```

✅ Output: `File not found`

---

## 🔹 7. Using `else` with `try-except`

```python
try:
    f = open('sample.txt', 'r')
except FileNotFoundError:
    print("File not found")
else:
    print(f.read())
```

✅ Output:

```
(If file exists): File content
(If file doesn't): File not found
```

---

## 🔹 8. Using `finally`

```python
try:
    f = open('sample.txt', 'r')
except FileNotFoundError:
    print("File not found")
finally:
    print("This always runs")
```

✅ Output:

```
File not found
This always runs
```

---

## 🔹 9. Raising Exceptions Manually

```python
raise ZeroDivisionError("Just testing")
```

❌ Output: `ZeroDivisionError: Just testing`

---

## 🔹 10. Custom Exception Class

```python
class MyException(Exception):
    def __init__(self, message):
        print(message)
```

### Using It:

```python
raise MyException("Something went wrong")
```

✅ Output:

```
Something went wrong
```

---

## 🔹 11. Real-World Example: Bank Class

```python
class Bank:
    def __init__(self, balance):
        self.balance = balance

    def withdraw(self, amount):
        if amount < 0:
            raise Exception("Amount cannot be negative")
        if self.balance < amount:
            raise Exception("Insufficient funds")
        self.balance -= amount

obj = Bank(10000)

try:
    obj.withdraw(15000)
except Exception as e:
    print(e)
else:
    print(obj.balance)
```

✅ Output: `Insufficient funds`

---

## 🔹 12. Example with Custom SecurityError

```python
class SecurityError(Exception):
    def __init__(self, message):
        print(message)

    def logout(self):
        print("logout")

class Google:
    def __init__(self, name, email, password, device):
        self.name = name
        self.email = email
        self.password = password
        self.device = device

    def login(self, email, password, device):
        if device != self.device:
            raise SecurityError("bhai teri to lag gayi")
        if email == self.email and password == self.password:
            print("welcome")
        else:
            print("login error")

obj = Google("nitish", "nitish@gmail.com", "1234", "android")

try:
    obj.login("nitish@gmail.com", "1234", "windows")
except SecurityError as e:
    e.logout()
else:
    print(obj.name)
finally:
    print("database connection closed")
```

✅ Output:

```
bhai teri to lag gayi
logout
database connection closed
```

---

## ✅ Final Summary and Best Practices

- Handle only expected exceptions.
    
- Use `finally` for cleanup (e.g., closing files).
    
- Prefer specific exceptions over general ones.
    
- Use custom exceptions for domain-specific errors.
    
- Avoid bare `except:` unless absolutely necessary.
    
- Always log errors in production apps.
    

> Exception handling makes your programs reliable and user-friendly.
