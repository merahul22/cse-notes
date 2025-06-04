
## 📁 File Handling in Python

### 🔹 What is File I/O?

File I/O (Input/Output) means reading from or writing data to a file stored on your computer.

---

### 🔸 Types of File Data:

|Type|Example|File Type|
|---|---|---|
|Text|`'12345'`|`.txt`, `.py`|
|Binary|`12345` as bytes|`.png`, `.mp3`, `.exe`|

---

### 🔹 Basic File Operations (Text Mode)

#### 1. **Opening a File**

```python
f = open('filename.txt', 'mode')
```

|Mode|Description|
|---|---|
|`'r'`|Read (default)|
|`'w'`|Write (overwrites if exists)|
|`'a'`|Append|
|`'rb'`|Read in binary|
|`'wb'`|Write in binary|

---

#### 2. **Writing to a File**

```python
f.write("Hello world")
f.writelines(['line1\n', 'line2\n'])
f.close()
```

#### 3. **Reading from a File**

```python
data = f.read()         # read all
data = f.read(10)       # first 10 characters
line = f.readline()     # one line at a time
lines = f.readlines()   # list of all lines
```

---

#### 4. **With Context Manager (`with` keyword)**

Automatically closes file.

```python
with open('file.txt', 'r') as f:
    data = f.read()
```

---

### 🔹 Cursor Management

- `f.seek(n)` – Move cursor to `n`th byte
    
- `f.tell()` – Current cursor position
    

---

## 🧊 Binary Files

Used for images, videos, etc.

```python
with open('image.png', 'rb') as f:
    data = f.read()
    
with open('copy.png', 'wb') as wf:
    wf.write(data)
```

---

## 📝 Writing Non-String Data

Direct writing of numbers/lists/dicts isn't allowed. Convert to string or use serialization.

```python
f.write(str(5))  # Good
f.write(5)       # ❌ Error
```

---

## 🌐 JSON Serialization

### 🔸 What is JSON?

**JavaScript Object Notation** – a human-readable format for storing structured data.

---

### 🔹 Serialization & Deserialization

- **Serialization** = Python → JSON
    
- **Deserialization** = JSON → Python
    

### 🔹 Using `json` Module

#### 1. Serialize (`json.dump`)

```python
import json

data = {"name": "Nitish", "age": 33}
with open('data.json', 'w') as f:
    json.dump(data, f, indent=4)
```

✅ `indent` makes it pretty and readable.

#### 2. Deserialize (`json.load`)

```python
with open('data.json', 'r') as f:
    data = json.load(f)
    print(data)  # Python dict
```

---

### 🔹 Works With:

- `dict`, `list`, `tuple`, `str`, `int`, `float`, `bool`
    

---

### 🔹 Custom Objects in JSON

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

def custom_encoder(obj):
    if isinstance(obj, Person):
        return {"name": obj.name, "age": obj.age}

p = Person("Nitish", 33)
with open("person.json", "w") as f:
    json.dump(p, f, default=custom_encoder, indent=4)
```

---

## 🧪 Pickle Module (Python-Specific Binary Format)

### 🔹 Pickling: Python → Binary

```python
import pickle

with open("data.pkl", "wb") as f:
    pickle.dump(p, f)
```

### 🔹 Unpickling: Binary → Python

```python
with open("data.pkl", "rb") as f:
    obj = pickle.load(f)
    obj.display_info()
```

---

### 🆚 JSON vs Pickle

|Feature|JSON|Pickle|
|---|---|---|
|Format|Text (Human-readable)|Binary|
|Language|Cross-language|Python-only|
|Performance|Slower|Faster for complex objects|
|Use-case|APIs, Web, Config|Machine learning models, etc.|

---

## ✅ Summary

|Topic|Key Idea|
|---|---|
|Text Files|Handle readable content|
|Binary Files|Handle media & executables|
|JSON|Data sharing & APIs|
|Pickle|Save/load Python objects|
