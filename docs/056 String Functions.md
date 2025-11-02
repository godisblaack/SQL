# 56 String Functions

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/nGNFiezVwB0?si=BoEsuRQRkVtX8_ZI"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🎯 1. Goal

Learn how to manipulate and clean text values using MySQL’s built-in string functions.

---

## ⚙️ 2. Common String Functions

| Function                        | Description                                                                                            | Example                                       | Result         |
| ------------------------------- | ------------------------------------------------------------------------------------------------------ | --------------------------------------------- | -------------- |
| **LENGTH(str)**                 | Returns the number of characters in `str`.                                                             | `LENGTH('sky')`                               | `3`            |
| **UPPER(str)**                  | Converts all characters to uppercase.                                                                  | `UPPER('sky')`                                | `SKY`          |
| **LOWER(str)**                  | Converts all characters to lowercase.                                                                  | `LOWER('Sky')`                                | `sky`          |
| **LTRIM(str)**                  | Removes spaces from the **left** side.                                                                 | `LTRIM('   sky')`                             | `sky`          |
| **RTRIM(str)**                  | Removes spaces from the **right** side.                                                                | `RTRIM('sky   ')`                             | `sky`          |
| **TRIM(str)**                   | Removes spaces from **both** sides.                                                                    | `TRIM('   sky   ')`                           | `sky`          |
| **LEFT(str, n)**                | Returns `n` characters from the **left**.                                                              | `LEFT('Kindergarten', 4)`                     | `Kind`         |
| **RIGHT(str, n)**               | Returns `n` characters from the **right**.                                                             | `RIGHT('Kindergarten', 6)`                    | `garten`       |
| **SUBSTRING(str, pos [, len])** | Returns characters from position `pos` (1-based index). If `len` omitted → returns till end.           | `SUBSTRING('Kindergarten', 3, 5)`             | `nderg`        |
| **LOCATE(substr, str)**         | Returns position of first occurrence of `substr` in `str`. Case-insensitive. Returns `0` if not found. | `LOCATE('n', 'Kindergarten')`                 | `3`            |
| **REPLACE(str, from, to)**      | Replaces all occurrences of `from` with `to`.                                                          | `REPLACE('Kindergarten', 'garten', 'garden')` | `Kindergarden` |
| **CONCAT(str1, str2, …)**       | Combines (joins) strings together.                                                                     | `CONCAT('first', 'last')`                     | `firstlast`    |

---

## 💻 3. Example Queries

```sql
SELECT LENGTH('sky')
UNION ALL
SELECT UPPER('sky')
UNION ALL
SELECT LOWER('Sky')
UNION ALL
SELECT LTRIM('    sky')
UNION ALL
SELECT RTRIM('sky    ')
UNION ALL
SELECT TRIM('   sky   ')
UNION ALL
SELECT LEFT('Kindergarten', 4)
UNION ALL
SELECT RIGHT('Kindergarten', 6)
UNION ALL
SELECT SUBSTRING('Kindergarten', 3, 5)
UNION ALL
SELECT SUBSTRING('Kindergarten', 3)
UNION ALL
SELECT LOCATE('n', 'Kindergarten')
UNION ALL
SELECT LOCATE('q', 'Kindergarten')
UNION ALL
SELECT LOCATE('garten', 'Kindergarten')
UNION ALL
SELECT REPLACE('Kindergarten', 'garten', 'garden')
UNION ALL
SELECT CONCAT('first', 'last');
```

### ✅ Example Output (simplified)

| result       |
| ------------ |
| 3            |
| SKY          |
| sky          |
| sky          |
| sky          |
| sky          |
| Kind         |
| garten       |
| nderg        |
| ndergarten   |
| 3            |
| 0            |
| 7            |
| Kindergarden |
| firstlast    |

---

## 🧩 4. Practical Example

Combine first and last names from the **customers** table:

```sql
USE sql_store;

SELECT
	CONCAT(first_name, ' ', last_name) AS full_name
FROM
	customers;
```

### ✅ Output

| full_name   |
| ----------- |
| John Smith  |
| Mary Lee    |
| David Brown |
| …           |

---

## 💡 5. Key Takeaways

| Tip                             | Description                                               |
| ------------------------------- | --------------------------------------------------------- |
| ✂️ `TRIM`, `LTRIM`, `RTRIM`     | Great for cleaning up messy user input.                   |
| 🔤 `UPPER`, `LOWER`             | Useful for case-insensitive comparisons.                  |
| 🧱 `LEFT`, `RIGHT`, `SUBSTRING` | Extract parts of strings.                                 |
| 🔍 `LOCATE`                     | Find the position of substrings (returns 0 if not found). |
| 🔄 `REPLACE`                    | Quickly modify text in your data.                         |
| 🔗 `CONCAT`                     | Build readable names, addresses, or identifiers.          |

---

## 🔗 6. Explore More

📘 Official Docs: [MySQL String Functions](https://dev.mysql.com/doc/refman/en/string-functions.html)

---

## 7. Queries from the video

```sql
SELECT LENGTH('sky')
UNION ALL
SELECT UPPER('sky')
UNION ALL
SELECT LOWER('Sky')
UNION ALL
SELECT LTRIM('    sky')
UNION ALL
SELECT RTRIM('sky    ')
UNION ALL
SELECT TRIM('   sky   ')
UNION ALL
SELECT LEFT('Kindergarten', 4)
UNION ALL
SELECT RIGHT('Kindergarten', 6)
UNION ALL
SELECT SUBSTRING('Kindergarten', 3, 5)
UNION ALL
SELECT SUBSTRING('Kindergarten', 3)
UNION ALL
SELECT LOCATE('n', 'Kindergarten')
UNION ALL
SELECT LOCATE('q', 'Kindergarten')
UNION ALL
SELECT LOCATE('garten', 'Kindergarten')
UNION ALL
SELECT REPLACE('Kindergarten', 'garten', 'garden')
UNION ALL
SELECT CONCAT('first', 'last');

USE sql_store;

SELECT
	CONCAT(first_name, ' ', last_name) AS full_name
FROM
	customers;
```