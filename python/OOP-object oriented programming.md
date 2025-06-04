## 🔹 What is OOP?

**Object-Oriented Programming** is a paradigm based on the concept of **"objects"** — instances that combine **data (attributes)** and **behavior (methods)**.
## 🔸 1. Basic Concepts

|Concept|Meaning|
|---|---|
|Class|Blueprint for creating objects|
|Object|Instance of a class|
|Attribute|Variable associated with an object|
|Method|Function associated with an object|
|Constructor|Special method to initialize objects (`__init__`)|
|`self`|Refers to the instance itself|

🔸 2. Creating a Class and Object
```
class Person:
    def __init__(self, name, age):  # constructor
        self.name = name
        self.age = age

    def greet(self):
        print(f"Hello, I am {self.name} and I am {self.age} years old.")

# Creating object
p1 = Person("Alice", 25)
p1.greet()

```

## 🔸 3. The `self` Keyword

- Refers to the **current instance** of the class.

- Needed to access instance variables and methods.

 🔸 4. Class vs Instance Attributes
`
```
class Dog:
    species = "Canine"  # class attribute

    def __init__(self, name):
        self.name = name  # instance attribute

dog1 = Dog("Rex")
dog2 = Dog("Bolt")

print(dog1.species)  # Canine
print(dog1.name)     # Rex

```

## 🔸 5. Types of Methods

|Method Type|Defined Using|Description|
|---|---|---|
|Instance|`def method(self)`|Operates on object (default)|
|Class|`@classmethod`|Operates on class (`cls`)|
|Static|`@staticmethod`|Doesn’t access `self` or `cls`|

```
class Example:
    @classmethod
    def cls_method(cls):
        print("Class method")

    @staticmethod
    def static_method():
        print("Static method")

```

# Example
```
class Fraction:

  # parameterized constructor

  def __init__(self,x,y):

    self.num = x

    self.den = y

  def __str__(self):

    return '{}/{}'.format(self.num,self.den)
    
  def __add__(self,other):

    new_num = self.num*other.den + other.num*self.den

    new_den = self.den*other.den
    
    return '{}/{}'.format(new_num,new_den)

  def __sub__(self,other):

    new_num = self.num*other.den - other.num*self.den

    new_den = self.den*other.den

    return '{}/{}'.format(new_num,new_den)

  def __mul__(self,other):

    new_num = self.num*other.num

    new_den = self.den*other.den
    
    return '{}/{}'.format(new_num,new_den)

  def __truediv__(self,other):

    new_num = self.num*other.den

    new_den = self.den*other.num

    return '{}/{}'.format(new_num,new_den)

  def convert_to_decimal(self):

    return self.num/self.den
```

## 🔍 Breakdown of the Class

```python
class Fraction:
```

You're creating a custom class named `Fraction`.

---

### 🔹 `__init__` – Constructor

```python
def __init__(self, x, y):
    self.num = x
    self.den = y
```

- This initializes a fraction with numerator `x` and denominator `y`.
    
- Example: `f1 = Fraction(2, 5)` → 2/5
    

---

### 🔹 `__str__` – String Representation

```python
def __str__(self):
    return '{}/{}'.format(self.num, self.den)
```

- This makes sure that when you `print(f1)`, you get `2/5` instead of something like `<__main__.Fraction object at 0x...>`
    

---

### 🔹 `__add__` – Addition

```python
def __add__(self, other):
    new_num = self.num * other.den + other.num * self.den
    new_den = self.den * other.den
    return '{}/{}'.format(new_num, new_den)
```

To add two fractions like `a/b + c/d`, we use the formula:

$$ab/+c/d=ad+bc/bd$$

### 🔹 `__sub__` – Subtraction

```python
def __sub__(self, other):
    new_num = self.num * other.den - other.num * self.den
    new_den = self.den * other.den
    return '{}/{}'.format(new_num, new_den)
```

$$a/b−c/d=ad−bc/bd$$
### 🔹 `__mul__` – Multiplication

```python
def __mul__(self, other):
    new_num = self.num * other.num
    new_den = self.den * other.den
    return '{}/{}'.format(new_num, new_den)
```

$$a/b⋅c/d=ac/bd$$

### 🔹 `__truediv__` – Division

```python
def __truediv__(self, other):
    new_num = self.num * other.den
    new_den = self.den * other.num
    return '{}/{}'.format(new_num, new_den)
```

$$a/b÷c/d=a⋅d/b⋅c$$

### 🔹 `convert_to_decimal` – Convert Fraction to Decimal

```python
def convert_to_decimal(self):
    return self.num / self.den
```

- Converts the fraction to a float.
    
- Example: `Fraction(1, 4).convert_to_decimal()` → `0.25`
    

---

## 🔄 Improvements (Optional)

1. **Return Fraction objects instead of strings** for operations:
    

```python
def __add__(self, other):
    new_num = self.num * other.den + other.num * self.den
    new_den = self.den * other.den
    return Fraction(new_num, new_den)
```

2. **Add simplification (reduce to lowest terms)** using `math.gcd()`:
    

```python
from math import gcd

def simplify(self):
    common = gcd(self.num, self.den)
    self.num //= common
    self.den //= common
```

3. **Handle zero denominator** in `__init__`.
    

---

## ✅ Example Usage

```python
f1 = Fraction(1, 2)
f2 = Fraction(1, 3)

print(f1 + f2)             # '5/6'
print(f1.convert_to_decimal())  # 0.5
```


### ✅ **1. Class for 2D Point and Distance Calculations**

```python
import math

class Point2D:
    origin = (0, 0)  # Static Variable

    def __init__(self, x, y):
        self.x = x
        self.y = y

    def display(self):
        print(f"Point: ({self.x}, {self.y})")

    def distance_to(self, other_point):
        return math.sqrt((self.x - other_point.x) ** 2 + (self.y - other_point.y) ** 2)

    def distance_from_origin(self):
        return math.sqrt(self.x ** 2 + self.y ** 2)
```

---

### ✅ **2. Line Class**

```python
class Line:
    def __init__(self, a, b, c):  # ax + by + c = 0
        self.a = a
        self.b = b
        self.c = c

    def is_point_on_line(self, point: Point2D):
        return self.a * point.x + self.b * point.y + self.c == 0

    def distance_from_point(self, point: Point2D):
        numerator = abs(self.a * point.x + self.b * point.y + self.c)
        denominator = math.sqrt(self.a**2 + self.b**2)
        return numerator / denominator
```

---

### ✅ **3. Object Creation & Attribute Access**

```python
# Creating points
p1 = Point2D(3, 4)
p2 = Point2D(6, 8)

# Accessing attributes
print(p1.x, p1.y)

# Adding attribute from outside class
p1.label = "A"
print(p1.label)
```

---

### ✅ **4. Reference Variables and Mutability**

```python
# Reference variables hold the same object
p3 = p1  # No new object created
p3.x = 10
print(p1.x)  # Output: 10, as p3 and p1 point to the same object

# Creating object without reference variable
print(Point2D(1, 2).distance_from_origin())  # Temporary object

# Multiple reference variables
a = p2
b = p2
a.y = 20
print(b.y)  # Output: 20
```

---

### ✅ **5. Pass by Reference & Mutability**

```python
def modify_point(point: Point2D):
    point.x += 1
    point.y += 1

modify_point(p1)
print(p1.x, p1.y)  # Values are changed — objects are mutable
```

---

### ✅ **6. Encapsulation Example**

```python
class SecurePoint:
    def __init__(self, x, y):
        self.__x = x  # Private
        self.__y = y  # Private

    def get_coords(self):
        return (self.__x, self.__y)

    def set_coords(self, x, y):
        self.__x = x
        self.__y = y
```

---

### ✅ **7. Collection of Objects**

```python
points = [Point2D(1, 2), Point2D(3, 4), Point2D(5, 6)]

for pt in points:
    pt.display()
```

---

### ✅ **8. Static vs Instance Variables**

```python
class Example:
    count = 0  # Static variable

    def __init__(self):
        Example.count += 1  # Accessed with ClassName
        self.id = Example.count  # Instance variable

a = Example()
b = Example()
c = Example()

print("Static count:", Example.count)  # Output: 3
print(a.id, b.id, c.id)  # Instance variables: 1 2 3
```

---

### ✅ **📌 Summary of Static Variables**

|Feature|Static Variable|Instance Variable|
|---|---|---|
|Scope|Class-level|Object-level|
|Accessed by|`ClassName.var` or `object`|`object.var`|
|Shared between objects|Yes|No|
|Memory|Allocated once|Allocated per object|

---

### ✅ Sample Usage

```python
# Coordinate example
p1 = Point2D(3, 4)
p2 = Point2D(6, 8)

print("Distance:", p1.distance_to(p2))
print("From origin:", p1.distance_from_origin())

# Line example
line = Line(1, -1, -2)
print("Is p1 on line?", line.is_point_on_line(p1))
print("Distance from line:", line.distance_from_point(p1))
```


