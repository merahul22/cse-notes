

# 📘 SQL  – Structured Query Language

---

## 1. 🔹 What is SQL?

**SQL (Structured Query Language)** is the standard language used to communicate with relational databases like MySQL, PostgreSQL, Oracle, and SQL Server.  
SQL allows you to **create**, **read**, **update**, and **delete** data (commonly referred to as CRUD operations).

---

## 2. 🔹 SQL Categories

|Category|Description|
|---|---|
|**DDL**|Data Definition Language (CREATE, ALTER, DROP)|
|**DML**|Data Manipulation Language (INSERT, UPDATE)|
|**DQL**|Data Query Language (SELECT)|
|**DCL**|Data Control Language (GRANT, REVOKE)|
|**TCL**|Transaction Control Language (COMMIT, ROLLBACK)|

---

## 3. 🔹 Basic SQL Syntax

```sql
-- Semicolon ends a statement
SELECT column1, column2 FROM table_name;
```

---

## 4. 🔹 DDL – Data Definition Language

### ✅ CREATE TABLE

```sql
CREATE TABLE Students (
    ID INT PRIMARY KEY,
    Name VARCHAR(100),
    Age INT,
    Grade CHAR(1)
);
```

### ✅ ALTER TABLE

```sql
ALTER TABLE Students ADD Email VARCHAR(100);
ALTER TABLE Students DROP COLUMN Grade;
```

### ✅ DROP TABLE

```sql
DROP TABLE Students;
```

---

## 5. 🔹 DML – Data Manipulation Language

### ✅ INSERT INTO

```sql
INSERT INTO Students (ID, Name, Age, Grade) VALUES (1, 'Alice', 15, 'A');
```

### ✅ UPDATE

```sql
UPDATE Students SET Age = 16 WHERE ID = 1;
```

### ✅ DELETE

```sql
DELETE FROM Students WHERE ID = 1;
```

---

## 6. 🔹 DQL – Data Query Language

### ✅ SELECT

```sql
SELECT * FROM Students;
SELECT Name, Age FROM Students WHERE Age > 15;
```

### ✅ WHERE Clause

```sql
SELECT * FROM Students WHERE Grade = 'A' AND Age < 17;
```

### ✅ ORDER BY

```sql
SELECT * FROM Students ORDER BY Age DESC;
```

### ✅ LIMIT

```sql
SELECT * FROM Students LIMIT 5;
```

---

## 7. 🔹 Filtering Data

### ✅ LIKE Operator (Pattern Matching)

```sql
SELECT * FROM Students WHERE Name LIKE 'A%'; -- starts with A
```

### ✅ IN Operator

```sql
SELECT * FROM Students WHERE Grade IN ('A', 'B');
```

### ✅ BETWEEN Operator

```sql
SELECT * FROM Students WHERE Age BETWEEN 14 AND 17;
```

---

## 8. 🔹 Aggregate Functions

|Function|Purpose|
|---|---|
|COUNT()|Number of rows|
|SUM()|Total sum|
|AVG()|Average value|
|MAX()|Maximum value|
|MIN()|Minimum value|

```sql
SELECT COUNT(*) FROM Students;
SELECT AVG(Age) FROM Students;
```

---

## 9. 🔹 GROUP BY and HAVING

```sql
SELECT Grade, COUNT(*) 
FROM Students 
GROUP BY Grade;

-- Filtering grouped results
SELECT Grade, COUNT(*) 
FROM Students 
GROUP BY Grade 
HAVING COUNT(*) > 2;
```

---

## 10. 🔹 JOINS in SQL

|Join Type|Description|
|---|---|
|INNER JOIN|Matching rows in both tables|
|LEFT JOIN|All rows from left + matched from right|
|RIGHT JOIN|All rows from right + matched from left|
|FULL OUTER JOIN|All rows when there is a match in one table|

```sql
-- INNER JOIN Example
SELECT A.Name, B.Course
FROM Students A
INNER JOIN Enrollments B ON A.ID = B.StudentID;
```

---

## 11. 🔹 Subqueries

```sql
-- Subquery in WHERE
SELECT Name 
FROM Students 
WHERE Age > (SELECT AVG(Age) FROM Students);
```

---

## 12. 🔹 Views

```sql
-- Creating a view
CREATE VIEW TeenStudents AS
SELECT * FROM Students WHERE Age BETWEEN 13 AND 19;

-- Using a view
SELECT * FROM TeenStudents;
```

---

## 13. 🔹 Indexes

```sql
CREATE INDEX idx_age ON Students(Age);
```

> Indexes speed up SELECT queries but slow down INSERT/UPDATE/DELETE.

---

## 14. 🔹 Transactions (TCL)

```sql
BEGIN;
UPDATE Students SET Grade = 'B' WHERE ID = 1;
COMMIT;

-- or ROLLBACK;
```

---

## 15. 🔹 SQL Best Practices

- Always backup data before major changes.
    
- Use `WHERE` clauses in UPDATE/DELETE to avoid accidental full-table updates.
    
- Use indexes wisely.
    
- Normalize database schema to reduce redundancy.



## 1. ER‑Style Overview

**Entities & Attributes**

- **Book**: ISBN (PK), Title, PublicationYear, PublisherID (FK)
    
- **Author**: AuthorID (PK), Name, Country
    
- **Publisher**: PublisherID (PK), Name, Address
    
- **Member**: MemberID (PK), Name, JoinDate, Email
    
- **Loan**: LoanID (PK), MemberID (FK), ISBN (FK), LoanDate, DueDate, ReturnDate
    

**Relationships**

- A **Book** can have **many** Authors (M:N) → resolved via **BookAuthor**
    
- A **Member** can have **many** Loans (1:N)
    
- A **Book** can appear in **many** Loans (1:N)
    

---

## 2. Relational Schema & DDL

```sql
-- Publishers
CREATE TABLE Publisher (
  PublisherID   INT        PRIMARY KEY,
  Name          VARCHAR(100) NOT NULL,
  Address       VARCHAR(255)
);

-- Authors
CREATE TABLE Author (
  AuthorID      INT        PRIMARY KEY,
  Name          VARCHAR(100) NOT NULL,
  Country       VARCHAR(50)
);

-- Books
CREATE TABLE Book (
  ISBN           CHAR(13)  PRIMARY KEY,
  Title          VARCHAR(200) NOT NULL,
  PublicationYear INT,
  PublisherID    INT        REFERENCES Publisher(PublisherID)
);

-- Junction for Book–Author M:N
CREATE TABLE BookAuthor (
  ISBN       CHAR(13) REFERENCES Book(ISBN),
  AuthorID   INT      REFERENCES Author(AuthorID),
  PRIMARY KEY (ISBN, AuthorID)
);

-- Library Members
CREATE TABLE Member (
  MemberID     INT         PRIMARY KEY,
  Name         VARCHAR(100) NOT NULL,
  JoinDate     DATE        DEFAULT CURRENT_DATE,
  Email        VARCHAR(100) UNIQUE
);

-- Loan Records
CREATE TABLE Loan (
  LoanID       INT         PRIMARY KEY,
  MemberID     INT         REFERENCES Member(MemberID),
  ISBN         CHAR(13)    REFERENCES Book(ISBN),
  LoanDate     DATE        DEFAULT CURRENT_DATE,
  DueDate      DATE        NOT NULL,
  ReturnDate   DATE        NULL
);
```

---

## 3. Sample Data Inserts

```sql
INSERT INTO Publisher VALUES
  (1, 'Penguin Random House', 'New York, USA'),
  (2, 'O’Reilly Media', 'Sebastopol, USA');

INSERT INTO Author VALUES
  (1, 'George Orwell', 'UK'),
  (2, 'Harper Lee', 'USA'),
  (3, 'Bjarne Stroustrup', 'Denmark');

INSERT INTO Book VALUES
  ('9780141036144','1984',1949,1),
  ('9780061120084','To Kill a Mockingbird',1960,1),
  ('9780321563842','The C++ Programming Language',2013,2);

INSERT INTO BookAuthor VALUES
  ('9780141036144',1),
  ('9780061120084',2),
  ('9780321563842',3);

INSERT INTO Member VALUES
  (100,'Alice Johnson','2025-01-10','alice@example.com'),
  (101,'Bob Smith','2025-02-05','bob@example.com');

INSERT INTO Loan VALUES
  (500,'100','9780141036144','2025-06-01','2025-06-15', NULL),
  (501,'101','9780321563842','2025-06-05','2025-06-19','2025-06-12');
```

---

## 4. Key Queries

### a) Create / Read / Update / Delete

```sql
-- Create: Add a new member
INSERT INTO Member (MemberID, Name, Email) VALUES (102,'Carol Lee','carol@example.com');

-- Read: List all overdue books
SELECT L.LoanID, M.Name, B.Title, DATEDIFF(CURRENT_DATE, L.DueDate) AS DaysOverdue
FROM Loan L
JOIN Member M ON L.MemberID = M.MemberID
JOIN Book B   ON L.ISBN     = B.ISBN
WHERE L.ReturnDate IS NULL
  AND L.DueDate < CURRENT_DATE;

-- Update: Record a return
UPDATE Loan
SET ReturnDate = CURRENT_DATE
WHERE LoanID = 500;

-- Delete: Remove a lost book record
DELETE FROM Book WHERE ISBN = '9780061120084';
```

### b) Joins & Aggregation

```sql
-- Which author has the most books?
SELECT A.Name, COUNT(*) AS BookCount
FROM Author A
JOIN BookAuthor BA ON A.AuthorID = BA.AuthorID
GROUP BY A.Name
ORDER BY BookCount DESC
LIMIT 1;

-- List members and total books currently on loan
SELECT M.Name, COUNT(*) AS BooksOnLoan
FROM Member M
LEFT JOIN Loan L ON M.MemberID = L.MemberID AND L.ReturnDate IS NULL
GROUP BY M.Name;
```

---

## 5. Transaction Example

Imagine checking out a book: you need to

1. Insert a loan
    
2. Decrement an inventory table (if you had one)
    
3. Commit only if both steps succeed
    

```sql
BEGIN;

INSERT INTO Loan (LoanID, MemberID, ISBN, DueDate)
  VALUES (502, 100, '9780321563842', '2025-06-22');

-- Suppose we had an Inventory table:
UPDATE Inventory
SET CopiesAvailable = CopiesAvailable - 1
WHERE ISBN = '9780321563842'
  AND CopiesAvailable > 0;

-- Check for any errors:
COMMIT;   -- Apply both changes atomically
-- ROLLBACK;  -- If something went wrong
```

