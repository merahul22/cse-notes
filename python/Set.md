

## 🔷 **1. What is a Set in Python?**

A **set** is an **unordered**, **mutable**, and **unindexed** collection of **unique elements** in Python.

> ✅ **Duplicates are automatically removed**, and **set operations** like union and intersection are supported.

---

## 🔸 **2. Characteristics of Sets**

|Characteristic|Description|
|---|---|
|✅ **Unordered**|No guaranteed order of elements|
|✅ **Mutable**|Can be modified after creation|
|✅ **Unique Elements**|Automatically removes duplicates|
|❌ **Unindexed**|No indexing or slicing allowed|
|✅ **Iterable**|Can loop over elements using `for` loop|

---

## 🛠️ **3. Creating a Set**

```python
s1 = {1, 2, 3}
s2 = set([1, 2, 2, 3])  # duplicates are removed
empty_set = set()       # correct way to create an empty set
```

---

## 🔍 **4. Accessing Items in a Set**

You **cannot access by index**, but you can loop:

```python
for item in s1:
    print(item)
```

---

## ✏️ **5. Editing Items in a Set**

Since sets are **unordered**, you **can’t directly edit** a specific item. You must remove and re-add:

```python
s1.remove(2)
s1.add(10)
print(s1)
```

---

## ➕ **6. Adding Items to a Set**

```python
s = {1, 2}
s.add(3)  # Add one item
s.update([4, 5], {6})  # Add multiple items
print(s)
```

---

## ❌ **7. Deleting Items from a Set**

```python
s = {1, 2, 3, 4}

s.remove(2)          # Removes 2, raises error if not present
s.discard(5)         # Safe remove; no error if 5 is not present
s.pop()              # Removes an arbitrary item
s.clear()            # Empties the set
del s                # Deletes the entire set
```

---

## 🔄 **8. Set Operations**

```python
a = {1, 2, 3}
b = {3, 4, 5}

print(a | b)   # Union → {1, 2, 3, 4, 5}
print(a & b)   # Intersection → {3}
print(a - b)   # Difference → {1, 2}
print(a ^ b)   # Symmetric Difference → {1, 2, 4, 5}
```

---

## 🔧 **9. Set Methods / Functions**

```python
s = {3, 1, 4, 1, 5}

print(len(s))        # 4
print(max(s))        # 5
print(min(s))        # 1
print(sum(s))        # 13
print(sorted(s))     # [1, 3, 4, 5]
```

---

## 🧊 **10. Frozenset**

A **frozenset** is an **immutable set** — it cannot be modified after creation.

```python
fs = frozenset([1, 2, 3, 2])
print(fs)

# fs.add(4) → ❌ Error: 'frozenset' object has no attribute 'add'
```

Used when:

- You need a set that is hashable (e.g., as a dictionary key)
    
- You want to protect data from changes
    

---

## 🧠 **11. Set Comprehension**

Like list comprehension but for sets:

```python
squared = {x*x for x in range(5)}
print(squared)  # {0, 1, 4, 9, 16}

evens = {x for x in range(10) if x % 2 == 0}
print(evens)    # {0, 2, 4, 6, 8}
```

---

## ✅ Summary Table

|Feature|Set|Frozenset|
|---|---|---|
|Mutable|✅ Yes|❌ No|
|Allows Duplicates|❌ No|❌ No|
|Indexed|❌ No|❌ No|
|Supports Operations|✅ Yes|✅ Yes|
|Hashable|❌ No|✅ Yes|
