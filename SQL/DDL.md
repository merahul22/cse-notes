

## 1. 🔹 What Is SQL?

- **SQL** (Structured Query Language) is the standard language for interacting with relational DBMSs.
    
- **Core Functionality**:
    
    - **DDL** – define and modify schema (tables, indexes, constraints)
        
    - **DML** – manipulate data (INSERT, UPDATE, DELETE)
        
    - **DQL** – query data (SELECT)
        
    - **DCL** – control access (GRANT, REVOKE)
        
    - **TCL** – manage transactions (COMMIT, ROLLBACK)
        

---

## 2. 🔹 Types of SQL Commands

|Category|Commands (non‑exhaustive)|Purpose|
|---|---|---|
|**DDL**|`CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME`|Define or change schema objects|
|**DML**|`INSERT`, `UPDATE`, `DELETE`, `MERGE`|Add or modify data|
|**DQL**|`SELECT`|Retrieve data|
|**DCL**|`GRANT`, `REVOKE`|Manage privileges|
|**TCL**|`COMMIT`, `ROLLBACK`, `SAVEPOINT`|Control transactions|

---

## 3. 🔹 Key SQL Functions (Built‑In)

- **Aggregate**: `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()`
    
- **String**: `CONCAT()`, `SUBSTR()`, `LOWER()`, `UPPER()`, `TRIM()`
    
- **Date/Time**: `NOW()`, `CURDATE()`, `DATEDIFF()`, `DATEADD()`
    
- **Conversion**: `CAST()`, `CONVERT()`
    
- **Conditional**: `COALESCE()`, `NULLIF()`, `CASE…WHEN`
    

---

## 4. 🔹 DDL Commands for Databases & Tables

### 4.1. Creating a Database

```sql
CREATE DATABASE SchoolDB
  CHARACTER SET utf8mb4
  COLLATE utf8mb4_unicode_ci;
```

### 4.2. Dropping a Database

```sql
DROP DATABASE IF EXISTS SchoolDB;
```

---

### 4.3. CREATE TABLE

```sql
CREATE TABLE Student (
  StudentID   INT         PRIMARY KEY,
  FirstName   VARCHAR(50) NOT NULL,
  LastName    VARCHAR(50) NOT NULL,
  BirthDate   DATE,
  Email       VARCHAR(100) UNIQUE,
  CreatedAt   TIMESTAMP   DEFAULT CURRENT_TIMESTAMP
);
```

- **NOT NULL**: disallows `NULL` values
    
- **UNIQUE**: enforces uniqueness
    
- **PRIMARY KEY**: combination of `NOT NULL` + `UNIQUE` + indexed
    

---

### 4.4. DROP TABLE

```sql
DROP TABLE IF EXISTS Student;
```

---

### 4.5. TRUNCATE TABLE

```sql
TRUNCATE TABLE Student;
```

- **Effect**: removes all rows quickly, resets identity counters
    
- **Transaction Behavior**: usually non‑logged/minimally logged, cannot be rolled back in some DBMS
    

---

### 4.6. RENAME TABLE

```sql
ALTER TABLE Student RENAME TO Learner;
```

_(Some dialects support `RENAME TABLE Student TO Learner;`)_

---

## 5. 🔹 Data Integrity & Methods to Ensure It

1. **Entity Integrity**
    
    - Each table must have a `PRIMARY KEY`, and its columns cannot be `NULL`.
        
2. **Referential Integrity**
    
    - `FOREIGN KEY` constraints ensure related rows exist in parent tables.
        
3. **Domain Integrity**
    
    - Data types, length, and formats restrict values (e.g., `VARCHAR(100)`, `DATE`).
        
4. **User‑Defined Integrity**
    
    - Business rules via `CHECK` constraints or triggers.
        

---

## 6. 🔹 Constraints & Referential Actions

|Constraint|Purpose|Syntax Example|
|---|---|---|
|**PRIMARY KEY**|Uniquely identifies each row|`PRIMARY KEY (StudentID)`|
|**FOREIGN KEY**|Enforces referential integrity|`FOREIGN KEY (DeptID) REFERENCES Department(DeptID)`|
|**UNIQUE**|Ensures all values in column(s) are distinct|`UNIQUE (Email)`|
|**NOT NULL**|Disallows `NULL`|`FirstName VARCHAR(50) NOT NULL`|
|**CHECK**|Validates values against a boolean expression|`CHECK (GPA BETWEEN 0 AND 4.0)`|
|**DEFAULT**|Provides default value when none is specified|`CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP`|

### 6.1. Referential Actions

When a parent row is updated/deleted, you control child behavior:

|Action|Effect|
|---|---|
|**CASCADE**|Propagate `DELETE`/`UPDATE` to children|
|**SET NULL**|Set child FK to `NULL`|
|**SET DEFAULT**|Set child FK to its default value|
|**RESTRICT**|Prevent operation if child exists|
|**NO ACTION**|Like `RESTRICT`, defers check to end of transaction|

```sql
CREATE TABLE Enrollment (
  StudentID   INT,
  CourseID    INT,
  EnrollDate  DATE,
  PRIMARY KEY (StudentID, CourseID),
  FOREIGN KEY (StudentID)
    REFERENCES Student(StudentID)
    ON DELETE CASCADE
    ON UPDATE NO ACTION
);
```

---

## 7. 🔹 ALTER TABLE Commands

### 7.1. Add a Column

```sql
ALTER TABLE Student
  ADD MiddleName VARCHAR(50) AFTER FirstName;
```

### 7.2. Drop a Column

```sql
ALTER TABLE Student
  DROP COLUMN MiddleName;
```

### 7.3. Modify / Change Column Definition

```sql
ALTER TABLE Student
  MODIFY COLUMN Email VARCHAR(150) NOT NULL;
```

_(In some systems use `ALTER COLUMN` instead of `MODIFY COLUMN`.)_

### 7.4. Rename a Column

```sql
ALTER TABLE Student
  RENAME COLUMN BirthDate TO DateOfBirth;
```

---

## 8. 🔹 Editing & Deleting Constraints

### 8.1. Add a Constraint

```sql
ALTER TABLE Student
  ADD CONSTRAINT chk_GPA
    CHECK (GPA BETWEEN 0.0 AND 4.0);
```

### 8.2. Drop a Constraint

```sql
ALTER TABLE Student
  DROP CONSTRAINT chk_GPA;
```

_(In MySQL you may need `DROP CHECK chk_GPA` or `ALTER TABLE Student DROP CHECK chk_GPA`.)_

### 8.3. Enable / Disable a Constraint

- **Oracle**:
    
    ```sql
    ALTER TABLE Student DISABLE CONSTRAINT fk_Department;
    ALTER TABLE Student ENABLE  CONSTRAINT fk_Department;
    ```
    
- **SQL Server**:
    
    ```sql
    ALTER TABLE Student NOCHECK CONSTRAINT ALL;
    ALTER TABLE Student CHECK CONSTRAINT ALL;
    ```
    

---

## 9. 🔹 Best Practices & Tips

- **Always backup** before dropping/truncating.
    
- Use `IF EXISTS`/`IF NOT EXISTS` to guard schema changes.
    
- Name constraints explicitly (e.g., `pk_Student`, `fk_Enrollment_Student`) for clarity.
    
- Group related `ALTER TABLE` changes in a single transaction where supported.
    
- Regularly review unused indexes and redundant constraints to optimize performance.
    