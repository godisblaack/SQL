# 17 The LIMIT Clause

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/uoxMoE7XmbE?si=82f2OBt0FmhrvuMv"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## Purpose of `LIMIT`

* The `LIMIT` clause restricts the **number of rows returned** by a query.
* Useful for:

  * Previewing data quickly.
  * Pagination in applications (showing records across multiple pages).
  * Returning **top-N results** (e.g., top 3 customers by loyalty points).

---

## Basic Usage

### Get First N Records

```sql
SELECT 
    *
FROM
    customers
LIMIT 3;
```

✅ Returns only the **first 3 customers** from the `customers` table.

---

### Exceeding Row Count

* If `LIMIT` is larger than the total rows in the table, MySQL simply returns **all rows**.

```sql
SELECT 
    *
FROM
    customers
LIMIT 300;
```

✅ Since there are only **10 customers**, this query returns all 10.

---

## Using `OFFSET` with `LIMIT`

* Syntax:

  ```sql
  LIMIT offset, row_count;
  ```
* `offset` → how many rows to **skip**
* `row_count` → how many rows to **fetch after skipping**

### Example: Pagination

Suppose we want **3 customers per page**:

* Page 1 → customers 1, 2, 3
* Page 2 → customers 4, 5, 6
* Page 3 → customers 7, 8, 9

Query for Page 3:

```sql
SELECT 
    *
FROM
    customers
LIMIT 6, 3;
```

✅ Skips the **first 6 rows**, then fetches the **next 3 rows** (customers 7, 8, 9).

---

## Exercise

**Goal:** Get top 3 loyal customers
**Steps:**

1. Select all customers.
2. Sort by `points` descending (`ORDER BY points DESC`).
3. Use `LIMIT 3` to return only the top 3.

```sql
SELECT 
    *
FROM
    customers
ORDER BY points DESC
LIMIT 3;
```

✅ `LIMIT` is often combined with `ORDER BY` to retrieve the **top N rows** based on some criteria.

✅ Sorts customers by `points` (loyalty points) in descending order, then picks the **top 3**.

---

## Important Notes

* **Order of clauses matters in SQL**:

  1. `SELECT`
  2. `FROM`
  3. `WHERE` (optional)
  4. `ORDER BY` (optional)
  5. `LIMIT`

⚠️ If you place `LIMIT` before `ORDER BY`, MySQL will throw an error.

---

## Key Takeaways

* Use `LIMIT N` to restrict results to the first N rows.
* Use `LIMIT offset, row_count` for pagination.
* Always combine with `ORDER BY` when you want **specific top-N records**.
* Clause order in SQL queries is strict → `LIMIT` always comes last.

---

## Queries from the video

```sql
USE sql_store;

SELECT 
    *
FROM
    customers
LIMIT 3;

-- If the argument passed with LIMIT clause is greater than the number of records that the query produces then we will get all the records in the qurey result.

-- The below example will output all the 10 customers in the table. We have 10 customers in our table not 300.
SELECT 
    *
FROM
    customers
LIMIT 300;

SELECT 
    *
FROM
    customers
LIMIT 6, 3;

-- Here 6 is the offset, meaning the query will skip the first 6 records from the query result and it will show the next 3 records that comes after first 6 records.
```

## Exercise

### Get Top 3 Loyal Customers
```sql
USE sql_store;

SELECT 
    *
FROM
    customers
ORDER BY points DESC
LIMIT 3;
```
