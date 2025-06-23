

## 1. Starting Point: Unnormalized “Order” Table

Suppose we have a “flat” orders table that captures order headers plus all line‑items in a single row:

|OrderID|CustomerName|Items|Qtys|Prices|OrderDate|
|:-:|:-:|:--|:--|:--|:--|
|1001|Alice|Widget, Gizmo|2, 1|9.99, 19.95|2025‑06‑01|
|1002|Bob|Widget|5|9.99|2025‑06‑02|
|1003|Alice|Thingamajig, Widget, Gizmo|1, 3, 2|4.50, 9.99,19.95|2025‑06‑03|

> **Issues:**
> 
> - **Non‑atomic columns** (`Items`, `Qtys`, `Prices` hold comma‑lists).
>     
> - **Updation/anomaly risk**: Impossible to update a single line‑item without string parsing.
>     

---

## 2. First Normal Form (1NF)

**Rule:** Eliminate repeating groups & ensure each column is atomic.

### 2.1. Table Structure After 1NF

We split out each line‑item into its own row:

|OrderID|CustomerName|Item|Qty|Price|OrderDate|
|:-:|:-:|:-:|:-:|:-:|:--|
|1001|Alice|Widget|2|9.99|2025‑06‑01|
|1001|Alice|Gizmo|1|19.95|2025‑06‑01|
|1002|Bob|Widget|5|9.99|2025‑06‑02|
|1003|Alice|Thingamajig|1|4.50|2025‑06‑03|
|1003|Alice|Widget|3|9.99|2025‑06‑03|
|1003|Alice|Gizmo|2|19.95|2025‑06‑03|

### 2.2. SQL to Create 1NF Table

```sql
CREATE TABLE Orders1NF (
  OrderID      INT      NOT NULL,
  CustomerName VARCHAR(100),
  Item         VARCHAR(50),
  Qty          INT,
  Price        DECIMAL(6,2),
  OrderDate    DATE,
  PRIMARY KEY (OrderID, Item)
);
```

---

## 3. Second Normal Form (2NF)

**Rule:** Eliminate partial dependencies: non‑key attributes must depend on the **entire** primary key.

In **Orders1NF**, `CustomerName` & `OrderDate` depend only on `OrderID`, not on the composite `(OrderID, Item)`.

### 3.1. Decompose into Two Tables

1. **Orders** (header data)
    
2. **OrderItems** (line items)
    

#### Orders (2NF)

|OrderID|CustomerName|OrderDate|
|:-:|:-:|:--|
|1001|Alice|2025‑06‑01|
|1002|Bob|2025‑06‑02|
|1003|Alice|2025‑06‑03|

#### OrderItems (2NF)

|OrderID|Item|Qty|Price|
|:-:|:-:|:-:|:-:|
|1001|Widget|2|9.99|
|1001|Gizmo|1|19.95|
|1002|Widget|5|9.99|
|1003|Thingamajig|1|4.50|
|1003|Widget|3|9.99|
|1003|Gizmo|2|19.95|

### 3.2. SQL to Create 2NF Tables

```sql
-- Order header
CREATE TABLE Orders (
  OrderID      INT AUTO_INCREMENT PRIMARY KEY,
  CustomerName VARCHAR(100),
  OrderDate    DATE
);

-- Order line-items
CREATE TABLE OrderItems (
  OrderID INT,
  Item    VARCHAR(50),
  Qty     INT,
  Price   DECIMAL(6,2),
  PRIMARY KEY (OrderID, Item),
  FOREIGN KEY (OrderID) REFERENCES Orders(OrderID)
);
```

---

## 4. Third Normal Form (3NF)

**Rule:** Eliminate transitive dependencies: non‑key attributes must depend **only** on the primary key, not on other non‑key attributes.

Suppose we have product details (e.g. price) repeating in **OrderItems**. Better to move product metadata into its own table.

### 4.1. Decompose into Three Tables

1. **Products**
    
2. **Orders**
    
3. **OrderItems**
    

#### Products (3NF)

|Item|UnitPrice|Category|
|:-:|:-:|:--|
|Widget|9.99|Hardware|
|Gizmo|19.95|Electronics|
|Thingamajig|4.50|Miscellaneous|

#### Orders (3NF)

|OrderID|CustomerName|OrderDate|
|:-:|:-:|:--|
|1001|Alice|2025‑06‑01|
|1002|Bob|2025‑06‑02|
|1003|Alice|2025‑06‑03|

#### OrderItems (3NF)

|OrderID|Item|Qty|
|:-:|:-:|:-:|
|1001|Widget|2|
|1001|Gizmo|1|
|1002|Widget|5|
|1003|Thingamajig|1|
|1003|Widget|3|
|1003|Gizmo|2|

### 4.2. SQL to Create 3NF Tables

```sql
CREATE TABLE Products (
  Item      VARCHAR(50) PRIMARY KEY,
  UnitPrice DECIMAL(6,2),
  Category  VARCHAR(50)
);

CREATE TABLE Orders (
  OrderID      INT AUTO_INCREMENT PRIMARY KEY,
  CustomerName VARCHAR(100),
  OrderDate    DATE
);

CREATE TABLE OrderItems (
  OrderID INT,
  Item    VARCHAR(50),
  Qty     INT,
  PRIMARY KEY (OrderID, Item),
  FOREIGN KEY (OrderID) REFERENCES Orders(OrderID),
  FOREIGN KEY (Item)    REFERENCES Products(Item)
);
```

---

## 5. Generating an ER Diagram in XAMPP/phpMyAdmin

You can visualize the above 3NF design with phpMyAdmin’s **Designer**:

1. **Install & Start XAMPP**
    
    - Download XAMPP for your OS
        
    - Run **Apache** & **MySQL** modules in the XAMPP Control Panel
        
2. **Create Database & Tables**
    
    - Open browser → `http://localhost/phpmyadmin/`
        
    - Click **New**, enter a database name (e.g. `shopdb`) → **Create**
        
    - Use **SQL** tab to paste & run the table‑creation statements from above
        
3. **Open the Designer**
    
    - In phpMyAdmin’s left pane, select your database (`shopdb`)
        
    - Click the **Designer** tab
        
    - phpMyAdmin will auto‑layout your tables and draw lines for each foreign‑key relationship
        
4. **Customize Layout**
    
    - Drag tables to reposition
        
    - Double‑click relationship lines to view or edit cardinality (1‑to‑many)
        
    - Use the toolbar to zoom in/out or export the diagram
        
5. **Export ER Diagram**
    
    - Click the **Export** icon (diskette) in Designer
        
    - Choose **PDF** or **PNG** to save a snapshot of your ER model
        

---

### Summary of Steps

1. **Normalize** your starting table step by step:
    
    - 1NF → atomic rows
        
    - 2NF → separate header vs. items
        
    - 3NF → isolate product metadata
        
2. **Implement** each stage with corresponding `CREATE TABLE` SQL.
    
3. **Use phpMyAdmin Designer** within XAMPP to auto‑generate and tweak an ER diagram.
