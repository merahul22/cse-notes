

## 📦 STRING DATA TYPES

|Type|Description|Max Length|
|---|---|---|
|`CHAR(n)`|Fixed-length string|0–255 chars|
|`VARCHAR(n)`|Variable-length string|0–65,535 bytes|
|`TEXT`|Large text (used for long strings)|65,535 chars|
|`MEDIUMTEXT`|Larger than TEXT|16 MB|
|`LONGTEXT`|Very large text|4 GB|

🔸 **CHAR vs VARCHAR**

- `CHAR(10)` always stores **exactly 10 characters**, padded with spaces.
    
- `VARCHAR(10)` only uses space needed for actual data.
    

---

## 🧾 WILDCARDS (Used with `LIKE`)

```sql
SELECT * FROM users WHERE name LIKE 'A%';     -- Starts with A
SELECT * FROM users WHERE name LIKE '%n';     -- Ends with n
SELECT * FROM users WHERE name LIKE '%an%';   -- Contains 'an'
SELECT * FROM users WHERE name LIKE '_a__';   -- Exactly 4-letter words with 2nd letter 'a'
```

---

## 🔤 STRING FUNCTIONS

### 1. `UPPER()` and `LOWER()`

```sql
SELECT UPPER('hello'), LOWER('WORLD');
-- Output: 'HELLO', 'world'
```

---

### 2. `CONCAT()` and `CONCAT_WS(separator, ...)`

```sql
SELECT CONCAT('Data', 'Science');              -- DataScience
SELECT CONCAT_WS('-', '2025', '06', '16');     -- 2025-06-16
```

---

### 3. `SUBSTR()` / `SUBSTRING()`

- Syntax: `SUBSTRING(str, start, length)`
    

```sql
SELECT SUBSTRING('abcdef', 2, 3);         -- Output: bcd
SELECT RIGHT('abcdef', 5);                -- Output: bcdef (last 5 characters)
```

---

### 4. `REPLACE()`

```sql
SELECT REPLACE('hello world', 'world', 'SQL');
-- Output: hello SQL
```

---

### 5. `REVERSE()` – check for palindrome

```sql
SELECT REVERSE('madam');    -- Output: madam
```

Check if palindrome:

```sql
SELECT CASE WHEN 'madam' = REVERSE('madam') THEN 'Yes' ELSE 'No' END AS is_palindrome;
```

---

### 6. `CHAR_LENGTH()` vs `LENGTH()`

```sql
SELECT CHAR_LENGTH('café'), LENGTH('café');
-- Output: 4, 5 (in UTF-8 encoding)
```

- `CHAR_LENGTH`: number of characters
    
- `LENGTH`: number of bytes (important for multi-byte characters)
    

---

### 7. `INSERT(str, pos, len, newstr)`

```sql
SELECT INSERT('abcdef', 2, 3, 'XYZ');
-- Output: aXYZef
```

---

### 8. `LEFT()` and `RIGHT()`

```sql
SELECT LEFT('abcdef', 3);    -- abc
SELECT RIGHT('abcdef', 2);   -- ef
```

---

### 9. `REPEAT()`

```sql
SELECT REPEAT('Ha', 3);      -- HaHaHa
```

---

### 10. `TRIM()`, `LTRIM()`, `RTRIM()`

```sql
SELECT TRIM('  SQL  ');      -- 'SQL'
SELECT LTRIM('  SQL');       -- 'SQL'
SELECT RTRIM('SQL  ');       -- 'SQL'
```

---

### 11. `SUBSTRING_INDEX(str, delimiter, count)`

- Split string by delimiter and return portion before/after.
    

```sql
SELECT SUBSTRING_INDEX('www.campusx.in', '.', 1);   -- www
SELECT SUBSTRING_INDEX('www.campusx.in', '.', -1);  -- in
```

---

### 12. `STRCMP(str1, str2)`

```sql
SELECT STRCMP('abc', 'bcd');   -- -1
SELECT STRCMP('abc', 'abc');   -- 0
SELECT STRCMP('bcd', 'abc');   -- 1
```

---

### 13. `LOCATE(substring, string)`

```sql
SELECT LOCATE('world', 'hello world');  -- 7
```

---

### 14. `LPAD()` and `RPAD()`

```sql
SELECT LPAD('hello', 10, '*');     -- *****hello
SELECT RPAD('hello', 10, '-');     -- hello-----
```

---

## ✅ Summary Table

|Function|Purpose|
|---|---|
|`UPPER/LOWER`|Convert case|
|`CONCAT/WS`|Combine strings|
|`SUBSTRING`|Extract substring|
|`REPLACE`|Replace part of a string|
|`REVERSE`|Reverse string|
|`CHAR_LENGTH`|Count characters|
|`LENGTH`|Count bytes|
|`INSERT`|Replace substring by position|
|`LEFT/RIGHT`|Get prefix/suffix|
|`REPEAT`|Repeat string|
|`TRIM/LTRIM/RTRIM`|Remove whitespace|
|`SUBSTRING_INDEX`|Extract part using delimiter|
|`STRCMP`|Compare two strings|
|`LOCATE`|Find position of substring|
|`LPAD/RPAD`|Pad strings|
