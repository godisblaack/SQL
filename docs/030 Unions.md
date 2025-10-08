# 30 Unions

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/uBARVeXTlKo?si=BvqYSacUGHcSlwIE"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Joins vs Union

* **Joins** → combine **columns** from multiple tables (side by side).
* **Union** → combine **rows** from multiple queries (stacked vertically).
* Both are powerful but solve **different problems**.

---

## 2. Example: Orders Table

```sql
USE sql_store;

SELECT * FROM orders;
```

* Orders table contains data across multiple years.
* Goal: label each order as **Active** (current year) or **Archived** (previous years).

---

## 3. Step 1 – Active Orders

```sql
SELECT 
    order_id, 
    order_date, 
    'Active' AS status
FROM orders
WHERE order_date >= '2019-01-01';
```

✔ Returns only current year orders.
✔ Adds a **new column** (`status`) with a string literal value.

---

## 4. Step 2 – Archived Orders

```sql
SELECT 
    order_id, 
    order_date, 
    'Archived' AS status
FROM orders
WHERE order_date < '2019-01-01';
```

✔ Returns past orders.
✔ Labels them as **Archived**.

---

## 5. Step 3 – Combine Using UNION

```sql
SELECT 
    order_id, order_date, 'Active' AS status
FROM orders
WHERE order_date >= '2019-01-01'

UNION

SELECT 
    order_id, order_date, 'Archived' AS status
FROM orders
WHERE order_date < '2019-01-01';
```

✔ Combines both result sets into a **single report**.
✔ UNION removes duplicates by default (use `UNION ALL` to keep duplicates).

---

## 6. UNION Between Different Tables

You can also combine rows from **different tables** if they return the **same number of columns** and compatible data types.

Example:

```sql
SELECT first_name
FROM customers
UNION
SELECT name
FROM shippers;
```

⚠️ Both queries return **1 column**, so UNION works.
⚠️ If column counts differ, MySQL throws an error.

```sql
-- ❌ ERROR: different number of columns
SELECT first_name, last_name
FROM customers
UNION
SELECT name
FROM shippers;
```

---

## 7. Column Naming Rule

* The **column names** in the final result come from the **first query**.
* Example:

```sql
SELECT name AS full_name FROM shippers
UNION
SELECT first_name FROM customers;
```

👉 Output column will be called **full\_name**.

---

## 8. Exercise – Customer Types Report

🎯 Classify customers into **Bronze, Silver, Gold** based on their points.

```sql
USE sql_store;

SELECT 
    customer_id, first_name, points, 'Bronze' AS type
FROM customers
WHERE points < 2000

UNION

SELECT 
    customer_id, first_name, points, 'Silver' AS type
FROM customers
WHERE points BETWEEN 2000 AND 3000

UNION

SELECT 
    customer_id, first_name, points, 'Gold' AS type
FROM customers
WHERE points > 3000

ORDER BY first_name;
```

✅ Returns:

* Bronze → points < 2000
* Silver → 2000 ≤ points ≤ 3000
* Gold → points > 3000
* Sorted by **first name**.

---

## 9. Key Takeaways

* **UNION** combines results from multiple queries.
* Each SELECT must:

  * Return the **same number of columns**.
  * Have **compatible data types** in each column position.
* Column names come from the **first query**.
* Use `UNION ALL` if you want to keep duplicates.

---

## 10. Queries form the video

```sql
USE sql_store;

SELECT 
    *
FROM
    orders;

SELECT 
    *
FROM
    orders
WHERE
    order_date >= '2019-01-01';

SELECT 
    order_id, order_date, 'Active' AS status
FROM
    orders
WHERE
    order_date >= '2019-01-01';

SELECT 
    order_id, order_date, 'Archived' AS status
FROM
    orders
WHERE
    order_date < '2019-01-01';

SELECT 
    order_id, order_date, 'Active' AS status
FROM
    orders
WHERE
    order_date >= '2019-01-01' 
UNION SELECT 
    order_id, order_date, 'Archived' AS status
FROM
    orders
WHERE
    order_date < '2019-01-01';
    
SELECT 
    first_name
FROM
    customers 
UNION SELECT 
    name
FROM
    shippers;
    
SELECT 
    first_name, last_name
FROM
    customers 
UNION SELECT 
    name
FROM
    shippers;
    
SELECT 
    name AS full_name
FROM
    shippers 
UNION SELECT 
    first_name
FROM
    customers;
```

## 11. Exercise

```sql
use sql_store;

SELECT 
    customer_id, first_name, points, 'Bronze' AS type
FROM
    customers
WHERE
    points < 2000 
UNION SELECT 
    customer_id, first_name, points, 'Silver' AS type
FROM
    customers
WHERE
    points BETWEEN 2000 AND 3000 
UNION SELECT 
    customer_id, first_name, points, 'Gold' AS type
FROM
    customers
WHERE
    points > 3000
ORDER BY first_name;
```

--- 

# 📘 SQL Joins – Notes

## 🔑 Types of Joins
- **INNER JOIN**
  - Returns only rows with matches in both tables.
  - Default join type if you just write `JOIN`.
  - Example: students who have exam records.

- **LEFT JOIN (LEFT OUTER JOIN)**
  - Returns all rows from the left table + matching rows from the right.
  - Non‑matches on the right → `NULL`.
  - Example: all students, even those with no exams.

- **RIGHT JOIN (RIGHT OUTER JOIN)**
  - Returns all rows from the right table + matching rows from the left.
  - Non‑matches on the left → `NULL`.
  - Example: all exam records, even if no matching student.

- **FULL OUTER JOIN**
  - Returns all rows from both tables.
  - Non‑matches on either side → `NULL`.
  - Example: complete picture of students + exams.

- **CROSS JOIN**
  - Returns Cartesian product (all combinations).
  - Same as writing `JOIN` without an `ON` condition.
  - Example: every student paired with every subject.

- **SELF JOIN**
  - A table joined with itself using aliases.
  - Used for comparisons within the same table.
  - Example: students who share the same subject.

---

## 📝 Quick Visual Summary
| Join Type       | Result |
|-----------------|--------|
| INNER JOIN      | Only matching rows |
| LEFT JOIN       | All left + matching right |
| RIGHT JOIN      | All right + matching left |
| FULL OUTER JOIN | All rows from both sides |
| CROSS JOIN      | All possible combinations |
| SELF JOIN       | Table joined with itself |

---

## 🔎 MySQL Specifics
- `JOIN` = `INNER JOIN` (default).
- `JOIN` without `ON` = Cartesian product (like `CROSS JOIN`).
- `SELF JOIN` is not a keyword — you create it by aliasing the same table twice.
- MySQL does **not auto‑detect** self joins or cross joins; you must write them explicitly.