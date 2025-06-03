

## 📖 **1. What is a Dictionary in Python?**

A **dictionary** is an **unordered**, **mutable** collection of **key-value pairs**, where each key must be unique and immutable.

---

## 📌 **2. Characteristics of a Dictionary**

|Feature|Description|
|---|---|
|✅ **Unordered**|Python 3.7+ maintains insertion order (but conceptually unordered)|
|✅ **Mutable**|Can be updated after creation|
|✅ **Key-Value Pairs**|Data stored as `key: value`|
|✅ **Unique Keys**|Duplicate keys are not allowed|
|✅ **Efficient Lookup**|Fast access using keys|

---

## 🧱 **3. Creating a Dictionary**

```python
d1 = {'name': 'Alice', 'age': 25}
d2 = dict(name='Bob', age=30)
d3 = dict([(1, 'a'), (2, 'b')])
empty = {}  # Empty dictionary
```

---

## 🔍 **4. Accessing Elements**

```python
d = {'a': 1, 'b': 2}
print(d['a'])         # 1
print(d.get('b'))     # 2
print(d.get('z'))     # None (safe access)
```

---

## ➕ **5. Adding Key-Value Pairs**

```python
d['c'] = 3
print(d)  # {'a': 1, 'b': 2, 'c': 3}
```

---

## 🧽 **6. Removing Key-Value Pairs**

```python
d.pop('b')         # Removes key 'b'
del d['a']         # Removes key 'a'
d.clear()          # Clears entire dictionary
```

---

## ✏️ **7. Editing/Updating Values**

```python
d = {'x': 1}
d['x'] = 100        # Update value
d.update({'y': 200})  # Add/update multiple keys
```

---

## 🔄 **8. Dictionary Operations**

```python
d = {'a': 1, 'b': 2}

print('a' in d)        # True
print(len(d))          # 2
print(list(d.keys()))  # ['a', 'b']
print(list(d.values())) # [1, 2]
print(list(d.items())) # [('a', 1), ('b', 2)]
```

---

## 🛠️ **9. Dictionary Functions & Methods**

|Function/Method|Description|
|---|---|
|`len(d)`|Number of key-value pairs|
|`d.keys()`|Returns view of keys|
|`d.values()`|Returns view of values|
|`d.items()`|Returns view of (key, value) pairs|
|`d.get(k)`|Returns value of key `k`, or `None` if missing|
|`d.pop(k)`|Removes and returns value for key `k`|
|`d.clear()`|Empties the dictionary|
|`d.update(dict2)`|Merges another dict into current one|
|`d.setdefault(k, v)`|Gets value of `k`, sets it to `v` if missing|
|`dict.fromkeys(seq, val)`|Creates new dict with keys from seq|

---

## 🧠 **10. Dictionary Comprehension**

```python
# Create a dictionary of squares
squares = {x: x**2 for x in range(5)}
print(squares)  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# Filtering with comprehension
evens = {k: v for k, v in squares.items() if v % 2 == 0}
print(evens)  # {0: 0, 2: 4, 4: 16}
```

---

## ✅ Example Summary

```python
person = {'name': 'Alice', 'age': 25}
print(person['name'])       # Access
person['email'] = 'a@x.com' # Add
person['age'] = 30          # Edit
person.pop('email')         # Remove
print(len(person))          # Functions
```
