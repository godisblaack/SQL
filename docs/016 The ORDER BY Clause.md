# 16 The ORDER BY Clause

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/-B4qSzjy_eo?si=cVJiSdIIbBe9rM1J"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## Default Sorting

* When selecting from a table **without `ORDER BY`**, results are usually sorted by the **primary key column**.
* Example: In the `customers` table, results are sorted by `customer_id` since it’s the **primary key**.
* **Primary Key Reminder:**

  * Uniquely identifies each row.
  * Must contain unique values.
  * Cannot contain `NULL`.

---

## Sorting with `ORDER BY`

### Ascending Order (Default)

* SQL sorts data in **ascending order (A → Z / 0 → 9)** by default.
* Example: Sort customers by first name:

```sql
SELECT 
    *
FROM
    customers
ORDER BY first_name;
```

---

### Descending Order

* Add the `DESC` keyword to reverse the sort order.

```sql
SELECT 
    *
FROM
    customers
ORDER BY first_name DESC;
```

---

### Multiple Columns

* You can sort by **more than one column**.
* Example: Sort customers first by `state`, then by `first_name` within each state.

```sql
SELECT 
    *
FROM
    customers
ORDER BY state, first_name;
```

* Add `DESC` to any column individually:

```sql
SELECT 
    *
FROM
    customers
ORDER BY state DESC, first_name ASC;
```

---

## Sorting by Non-Selected Columns

* In MySQL, you can sort results by **columns not included in the `SELECT` list**.
* Example: Select only names, but sort by `birth_date`.

```sql
SELECT 
    first_name, last_name
FROM
    customers
ORDER BY birth_date;
```

⚠️ Note: Some databases (like SQL Server, Oracle) don’t allow this.

---

## Sorting by Aliases

* You can use **aliases** (from the `SELECT` clause) in your `ORDER BY`.

```sql
SELECT 
    first_name, last_name, 10 AS points
FROM
    customers
ORDER BY points, first_name;
```

---

## Sorting by Expressions

* You can use arithmetic expressions directly in `ORDER BY`.
* Example: Sorting order items by total price (calculated on the fly).

```sql
SELECT 
    *, quantity * unit_price AS total_price
FROM
    order_items
WHERE
    order_id = 2
ORDER BY total_price DESC;
```

✅ More **readable** and avoids repeating the calculation.

---

## Sorting by Column Position (⚠️ Avoid)

* You can sort by column numbers in the `SELECT` list:

```sql
SELECT 
    first_name, last_name
FROM
    customers
ORDER BY 1, 2;
```

* Here `1` = `first_name`, `2` = `last_name`.

⚠️ **Why avoid this?**

* If the order of columns changes in `SELECT`, the sorting will change unexpectedly.
* Always **use column names** instead.

✅ Recommended:

```sql
SELECT 
    first_name, last_name
FROM
    customers
ORDER BY first_name, last_name;
```

---

## Exercise – Sorting Orders by Total Price

### Problem

Get all items from `order_items` where `order_id = 2`, sorted by total price (`quantity * unit_price`) in **descending order**.

---

### Solution 1: Without Alias

```sql
SELECT 
    *
FROM
    order_items
WHERE
    order_id = 2
ORDER BY quantity * unit_price DESC;
```

---

### Solution 2: With Calculated Column (Alias)

```sql
SELECT 
    *, quantity * unit_price AS total_price
FROM
    order_items
WHERE
    order_id = 2
ORDER BY total_price DESC;
```

✅ **Best practice:** Use the alias in `ORDER BY` to improve readability and avoid repeating calculations.

---

## Key Takeaways

* Use `ORDER BY` to control the sort order explicitly.
* Default sorting is usually by the **primary key**.
* You can:

  * Sort ascending (`ASC`, default) or descending (`DESC`).
  * Sort by multiple columns.
  * Sort by columns not in `SELECT` (MySQL allows this).
  * Sort by expressions or aliases.
* ❌ Avoid sorting by column positions (e.g., `ORDER BY 1, 2`).

---

## Queries from the video

```sql
USE sql_store;

SELECT 
    *
FROM
    customers
ORDER BY state DESC , first_name DESC;
-- ORDER BY state DESC , first_name;
-- ORDER BY state , first_name;
-- ORDER BY first_name DESC;
-- ORDER BY first_name;

SELECT 
    first_name, last_name
FROM
    customers
ORDER BY birth_date;


SELECT 
    first_name, last_name, 10 AS points
FROM
    customers
ORDER BY points , first_name;


SELECT 
    first_name, last_name
FROM
    customers
ORDER BY 1 , 2;
	-- Avoid this type of sorting, because on changing the columns on SELECT line the sorting order will also change. Always use columns name for sorting, as shown below.

SELECT 
    first_name, last_name
FROM
    customers
ORDER BY first_name, last_name;

-- Table is order by customer_id by default because it is the primary key. Values in the primary key column are unique and they identify every rows uniquely.
```

## Exercise 

### Orders with `order_id = 2` Sorted by Total Price (Descending)

### Without Alias
```sql
USE sql_store;

SELECT 
    *
FROM
    order_items
WHERE
    order_id = 2
ORDER BY quantity * unit_price DESC;
```

### With Calculated Column
```sql
USE sql_store;

SELECT 
    *, quantity * unit_price AS total_price
FROM
    order_items
WHERE
    order_id = 2
ORDER BY quantity * unit_price DESC;
```

### Using Alias in `ORDER BY`
```sql
USE sql_store;

SELECT 
    *, quantity * unit_price AS total_price
FROM
    order_items
WHERE
    order_id = 2
ORDER BY total_price DESC;
```
- **Benefit:** Improves readability and avoids repeating the calculation.
