# 🧹 SQL Data Cleaning Notes: Laptops Dataset

## 1. 📁 Create a Backup Table

```sql
CREATE TABLE laptops_backup LIKE laptops;
INSERT INTO laptops_backup SELECT * FROM laptops;
```

🔹 `CREATE TABLE LIKE` duplicates the schema.  
🔹 `INSERT INTO SELECT *` copies all records — important for safety before cleaning.

---

## 2. 🔍 Check Number of Rows

```sql
SELECT COUNT(*) FROM laptops;
```

🔹 Helps verify data size before/after operations.

---

## 3. 💾 Check Memory Consumption

```sql
SELECT DATA_LENGTH/1024 AS size_kb
FROM information_schema.TABLES
WHERE TABLE_SCHEMA = 'sql_cx_live'
AND TABLE_NAME = 'laptops';
```

🔹 Retrieves size in KB from system metadata. `DATA_LENGTH` gives total bytes.

---

## 4. 🧺 Drop Non-Important Columns

```sql
ALTER TABLE laptops DROP COLUMN `Unnamed: 0`;
```

🔹 Often `Unnamed: 0` is an index column from CSV. Dropping saves space.

---

## 5. 🗑️ Drop Rows with All NULLs

```sql
DELETE FROM laptops
WHERE `index` IN (
  SELECT `index` FROM laptops
  WHERE Company IS NULL AND TypeName IS NULL AND Inches IS NULL
  AND ScreenResolution IS NULL AND Cpu IS NULL AND Ram IS NULL
  AND Memory IS NULL AND Gpu IS NULL AND OpSys IS NULL
  AND Weight IS NULL AND Price IS NULL
);
```

🔹 Deletes rows with NULL in every field. Important to clean noisy data.

---

## 6. 🧠 Clean and Convert RAM

```sql
UPDATE laptops l1
SET Ram = (SELECT REPLACE(Ram,'GB','') FROM laptops l2 WHERE l2.index = l1.index);
ALTER TABLE laptops MODIFY COLUMN Ram INTEGER;
```

🔹 `REPLACE` removes 'GB'.  
🔹 We then change the column type to `INTEGER` for math operations.

---

## 7. ⚖️ Clean Weight Column

```sql
UPDATE laptops l1
SET Weight = (SELECT REPLACE(Weight,'kg','') FROM laptops l2 WHERE l2.index = l1.index);
```

🔹 Strips 'kg' unit for numeric analysis.

---

## 8. 💰 Round and Convert Price

```sql
UPDATE laptops l1
SET Price = (SELECT ROUND(Price) FROM laptops l2 WHERE l2.index = l1.index);
ALTER TABLE laptops MODIFY COLUMN Price INTEGER;
```

🔹 Rounds prices to nearest integer and optimizes storage.

---

## 9. 💻 Normalize Operating Systems

```sql
UPDATE laptops
SET OpSys =
  CASE
    WHEN OpSys LIKE '%mac%' THEN 'macos'
    WHEN OpSys LIKE 'windows%' THEN 'windows'
    WHEN OpSys LIKE '%linux%' THEN 'linux'
    WHEN OpSys = 'No OS' THEN 'N/A'
    ELSE 'other'
  END;
```

🔹 Standardizes OS values using `LIKE` pattern matching.

---

## 10. 🎮 GPU Brand and Model

```sql
ALTER TABLE laptops ADD COLUMN gpu_brand VARCHAR(255), ADD COLUMN gpu_name VARCHAR(255);
UPDATE laptops l1 SET gpu_brand = (SELECT SUBSTRING_INDEX(Gpu,' ',1) FROM laptops l2 WHERE l2.index = l1.index);
UPDATE laptops l1 SET gpu_name = (SELECT REPLACE(Gpu,gpu_brand,'') FROM laptops l2 WHERE l2.index = l1.index);
ALTER TABLE laptops DROP COLUMN Gpu;
```

🔹 `SUBSTRING_INDEX` extracts the brand.  
🔹 `REPLACE` removes brand from full name to get the model.

---

## 11. 🧠 CPU Brand, Name, and Speed

```sql
ALTER TABLE laptops ADD COLUMN cpu_brand VARCHAR(255), cpu_name VARCHAR(255), cpu_speed DECIMAL(10,1);

UPDATE laptops l1 SET cpu_brand = (SELECT SUBSTRING_INDEX(Cpu,' ',1) FROM laptops l2 WHERE l2.index = l1.index);

UPDATE laptops l1 SET cpu_speed = (
  SELECT CAST(REPLACE(SUBSTRING_INDEX(Cpu,' ',-1),'GHz','') AS DECIMAL(10,2))
  FROM laptops l2 WHERE l2.index = l1.index);

UPDATE laptops l1 SET cpu_name = (
  SELECT REPLACE(REPLACE(Cpu,cpu_brand,''),SUBSTRING_INDEX(REPLACE(Cpu,cpu_brand,''),' ',-1),'')
  FROM laptops l2 WHERE l2.index = l1.index);

ALTER TABLE laptops DROP COLUMN Cpu;
```

🔹 Extracts `brand`, `speed`, and `name` by breaking down CPU text.

---

## 12. 🖥️ Screen Resolution to Width & Height

```sql
ALTER TABLE laptops ADD COLUMN resolution_width INTEGER, ADD COLUMN resolution_height INTEGER;

UPDATE laptops
SET resolution_width = SUBSTRING_INDEX(SUBSTRING_INDEX(ScreenResolution,' ',-1),'x',1),
    resolution_height = SUBSTRING_INDEX(SUBSTRING_INDEX(ScreenResolution,' ',-1),'x',-1);
```

🔹 Uses nested `SUBSTRING_INDEX` to split `WIDTHxHEIGHT` values.

---

## 13. 🖱️ Touchscreen Flag

```sql
ALTER TABLE laptops ADD COLUMN touchscreen INTEGER;

UPDATE laptops SET touchscreen = ScreenResolution LIKE '%Touch%';
ALTER TABLE laptops DROP COLUMN ScreenResolution;
```

🔹 Returns 1 if the screen is touch-enabled, else 0.

---

## 14. ✏️ Refine CPU Name

```sql
UPDATE laptops SET cpu_name = SUBSTRING_INDEX(TRIM(cpu_name),' ',2);
```

🔹 Trims CPU name to 2 keywords (e.g., `Core i5`).

---

## 15. 💽 Memory Type and Storage Breakdown

```sql
ALTER TABLE laptops
ADD COLUMN memory_type VARCHAR(255),
ADD COLUMN primary_storage INTEGER,
ADD COLUMN secondary_storage INTEGER;

-- Identify Memory Type
UPDATE laptops SET memory_type = CASE
  WHEN Memory LIKE '%SSD%' AND Memory LIKE '%HDD%' THEN 'Hybrid'
  WHEN Memory LIKE '%SSD%' THEN 'SSD'
  WHEN Memory LIKE '%HDD%' THEN 'HDD'
  WHEN Memory LIKE '%Flash Storage%' THEN 'Flash Storage'
  ELSE NULL END;

-- Split memory sizes
UPDATE laptops
SET primary_storage = REGEXP_SUBSTR(SUBSTRING_INDEX(Memory,'+',1),'[0-9]+'),
    secondary_storage = CASE WHEN Memory LIKE '%+%' THEN REGEXP_SUBSTR(SUBSTRING_INDEX(Memory,'+',-1),'[0-9]+') ELSE 0 END;

-- Convert TB to GB
UPDATE laptops
SET primary_storage = CASE WHEN primary_storage <= 2 THEN primary_storage*1024 ELSE primary_storage END,
    secondary_storage = CASE WHEN secondary_storage <= 2 THEN secondary_storage*1024 ELSE secondary_storage END;
```

🔹 Classifies storage type.  
🔹 Splits `Memory` field into main and secondary.  
🔹 Converts TB to GB (e.g., 1TB → 1024 GB).

---

## ✅ Final Table Check

```sql
SELECT * FROM laptops;
```

🔹 Final check to verify all cleaned columns and new structure.

 refer to this dataset=[dataset](https://www.kaggle.com/datasets/ehtishamsadiq/uncleaned-laptop-price-dataset)
