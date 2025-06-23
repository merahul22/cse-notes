

## 1. Numeric Types

|Type|Storage|Range / Precision|Notes|
|---|---|---|---|
|**TINYINT**|1 byte|Signed: –128 to 127Unsigned: 0 to 255|Good for small enumerations or flags|
|**SMALLINT**|2 bytes|Signed: –32 768 to 32 767Unsigned: 0 to 65 535|When TINYINT is too small|
|**INT**|4 bytes|Signed: –2 147 483 648 to 2 147 483 647Unsigned: 0 to 4 294 967 295|Default integer type|
|**BIGINT**|8 bytes|Signed: –9.22×10¹⁸ to 9.22×10¹⁸Unsigned: 0 to 1.84×10¹⁹|For very large whole numbers|
|**DECIMAL(M,D)**|Variable (≈ M/2 bytes)|Exact: M total digits, D decimals|Financial data—exact arithmetic|
|**FLOAT(p)**|4 bytes|Approximate, single‑precision|Faster but inexact|
|**DOUBLE(p)**|8 bytes|Approximate, double‑precision|Higher precision than FLOAT|

### Examples

```sql
CREATE TABLE products (
  id             INT            UNSIGNED      AUTO_INCREMENT PRIMARY KEY,
  stock_quantity TINYINT        UNSIGNED      NOT NULL DEFAULT 0,
  reorder_level  SMALLINT       NOT NULL,
  price          DECIMAL(10,2)  NOT NULL,        -- up to 8 digits before decimal, 2 after
  weight_kg      FLOAT          NULL,
  rating         DOUBLE         NULL
);
```

---

## 2. Text Types

|Type|Max Length|Storage|Notes|
|---|---|---|---|
|**CHAR(n)**|0 to 255|Fixed n bytes (plus length byte)|Pads with spaces; very fast fixed‑width|
|**VARCHAR(n)**|0 to 65 535|Actual length + 1–2 bytes for length|Variable‑length; most common for text|
|**TINYTEXT**|255 bytes|Actual length + 1 byte|Small text blobs|
|**TEXT**|65 535 bytes|Actual length + 2 bytes|Standard large text|
|**MEDIUMTEXT**|16 777 215 bytes|Actual length + 3 bytes|Very large text|
|**LONGTEXT**|4 294 967 295 bytes|Actual length + 4 bytes|Maximum text|
|**BLOB** variants|Same sizes as TEXT types|Binary-safe equivalents of TINYTEXT, TEXT, etc.|Store binary data (images, etc.)|
|**ENUM(...)**|1–2 bytes|List of permitted values; stored internally as integers|Use for small sets of known values|
|**SET(...)**|1–8 bytes|Stored as bitmaps of permitted values|Multiple choice from a fixed list|

### Examples

```sql
CREATE TABLE users (
  username    VARCHAR(50)    NOT NULL,
  password    CHAR(60)       NOT NULL,    -- e.g. bcrypt hashes
  profile     TEXT           NULL,
  bio         MEDIUMTEXT     NULL,
  avatar      BLOB           NULL,
  status      ENUM('active','pending','banned') NOT NULL DEFAULT 'pending',
  tags        SET('sports','music','coding','travel') NULL
);
```

---

## 3. Date & Time Types

|Type|Storage|Range|Notes|
|---|---|---|---|
|**DATE**|3 bytes|'1000-01-01' to '9999-12-31'|Stores only date (Y‑M‑D)|
|**TIME**|3 bytes|'-838:59:59' to '838:59:59'|Time of day or elapsed time|
|**DATETIME**|5 bytes|'1000-01-01 00:00:00' to '9999-12-31 23:59:59'|No timezone conversion|
|**TIMESTAMP**|4 bytes|'1970-01-01 00:00:01' UTC to '2038-01-19 03:14:07' UTC|Auto‑update on INSERT/UPDATE if configured|
|**YEAR**|1 byte|1901 to 2155 (or 0000)|Store year only; 2‑ or 4‑digit format|

### Examples

```sql
CREATE TABLE events (
  event_id      INT           AUTO_INCREMENT PRIMARY KEY,
  title         VARCHAR(100)  NOT NULL,
  start_date    DATE          NOT NULL,
  start_time    TIME          NOT NULL,
  created_at    DATETIME      DEFAULT CURRENT_TIMESTAMP,
  updated_at    TIMESTAMP     DEFAULT CURRENT_TIMESTAMP 
                                ON UPDATE CURRENT_TIMESTAMP,
  event_year    YEAR          AS (YEAR(start_date))       -- generated column
);
```

---

## 4. Miscellaneous Types

|Type|Storage|Notes|
|---|---|---|
|**JSON**|Variable|Enforces valid JSON; supports indexing on JSON paths|
|**Spatial**|Variable|GIS/geometric; e.g. POINT, LINESTRING, POLYGON, GEOMETRYCOLLECTION, etc.|

### JSON Example

```sql
CREATE TABLE orders (
  order_id    INT           AUTO_INCREMENT PRIMARY KEY,
  customer    VARCHAR(100)  NOT NULL,
  metadata    JSON          NULL           -- arbitrary key/value data
);
-- Querying JSON:
SELECT
  JSON_EXTRACT(metadata, '$.couponCode') AS coupon
FROM orders
WHERE JSON_CONTAINS(metadata, '"VIP"', '$.tags');
```

### Spatial Example

```sql
CREATE TABLE landmarks (
  id       INT             AUTO_INCREMENT PRIMARY KEY,
  name     VARCHAR(100),
  location POINT           NOT NULL,
  area     POLYGON         NULL
);
-- Insert a point and polygon:
INSERT INTO landmarks (name, location, area)
VALUES
  ('Museum Plaza', ST_GeomFromText('POINT(77.1025 28.7041)')),
  ('Park Grounds', ST_GeomFromText(
     'POLYGON((77.1 28.7, 77.2 28.7, 77.2 28.8, 77.1 28.8, 77.1 28.7))'
  ));
-- Spatial query: find landmarks within a given polygon:
SELECT name
FROM landmarks
WHERE ST_Within(location,
    ST_GeomFromText('POLYGON((77.0 28.6, 77.3 28.6, 77.3 28.8, 77.0 28.8, 77.0 28.6))')
);
```

---

### Tips & Best Practices

- **Choose exact vs. approximate**  
    Use **DECIMAL** for money; **FLOAT/DOUBLE** for scientific data.
    
- **Length & storage**  
    Over‑allocating VARCHAR wastes memory/IO; use TEXT only when > 65 535 bytes.
    
- **Timezones**  
    TIMESTAMP auto‑converts to UTC; DATETIME does not—pick based on your needs.
    
- **JSON & spatial**  
    Leverage native JSON and GIS support for semi‑structured or geographic data.
    