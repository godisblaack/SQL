# 24 Outer Joins

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/r7DaTkbEpW8?si=LvuA5ONDUNoHWfKq"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Inner Join (Default)

By default, `JOIN` (or `INNER JOIN`) returns only the rows where **matching records exist in both tables**.

```sql
USE sql_store;

SELECT 
    c.customer_id, 
    c.first_name, 
    o.order_id
FROM customers c
JOIN orders o 
    ON c.customer_id = o.customer_id
ORDER BY c.customer_id;
```

* Only customers **with orders** appear in the result.
* Customers without orders are **excluded**.

---

## 2. Left Join (Left Outer Join)

`LEFT JOIN` ensures that **all rows from the left table** are returned, even if there’s no match in the right table.

```sql
SELECT 
    c.customer_id, 
    c.first_name, 
    o.order_id
FROM customers c
LEFT JOIN orders o 
    ON c.customer_id = o.customer_id
ORDER BY c.customer_id;
```

* All customers are shown.
* If a customer has **no orders**, `order_id` = `NULL`.
* ✅ Useful for finding **customers without orders**.

---

## 3. Right Join (Right Outer Join)

`RIGHT JOIN` ensures that **all rows from the right table** are returned, even if there’s no match in the left table.

```sql
SELECT 
    c.customer_id, 
    c.first_name, 
    o.order_id
FROM customers c
RIGHT JOIN orders o 
    ON c.customer_id = o.customer_id
ORDER BY c.customer_id;
```

* All orders are shown.
* Customers without orders do **not** appear.
* Equivalent to swapping table order with `LEFT JOIN`.

---

## 4. Swapping Table Order

If you want **all customers** but mistakenly wrote a `RIGHT JOIN`, swap the table order:

```sql
SELECT 
    c.customer_id, 
    c.first_name, 
    o.order_id
FROM orders o
RIGHT JOIN customers c 
    ON c.customer_id = o.customer_id
ORDER BY c.customer_id;
```

* Now all customers appear (same as `LEFT JOIN`).

---

## 5. Outer Keyword (Optional)

* You may see `LEFT OUTER JOIN` or `RIGHT OUTER JOIN`.
* The `OUTER` keyword is **optional** → just `LEFT JOIN` or `RIGHT JOIN` works fine.

---

## 6. Key Differences Recap

| Join Type    | Returns                                                                  |
| ------------ | ------------------------------------------------------------------------ |
| `INNER JOIN` | Only rows with matches in both tables                                    |
| `LEFT JOIN`  | All rows from left table + matching rows from right (non-matches → NULL) |
| `RIGHT JOIN` | All rows from right table + matching rows from left (non-matches → NULL) |

---

## 7. Exercise – Products and Order Items

We want a report that shows all products, including those that were **never ordered**.

```sql
USE sql_store;

SELECT 
    p.product_id, 
    p.name, 
    oi.quantity
FROM products p
LEFT JOIN order_items oi 
    ON p.product_id = oi.product_id
ORDER BY p.product_id;
```

* `LEFT JOIN` ensures all products are shown.
* If a product was never ordered → `quantity = NULL`.
* Example: `product_id = 7` appears even without any orders.

---

⚡ **Rule of thumb**:

* Use **INNER JOIN** when you only care about matching records.
* Use **LEFT JOIN / RIGHT JOIN** when you need **all rows** from one table, regardless of matches.

---

## Queries from the video

```sql
USE sql_store;

SELECT 
    c.customer_id, c.first_name, o.order_id
FROM
    customers c
        JOIN
    orders o ON c.customer_id = o.customer_id
ORDER BY c.customer_id;

SELECT 
    c.customer_id, c.first_name, o.order_id
FROM
    customers c
        LEFT JOIN
        -- LEFT OUTER JOIN
    orders o ON c.customer_id = o.customer_id
ORDER BY c.customer_id;

SELECT 
    c.customer_id, c.first_name, o.order_id
FROM
    customers c
        RIGHT JOIN
        -- RIGHT OUTER JOIN
    orders o ON c.customer_id = o.customer_id
ORDER BY c.customer_id;

SELECT 
    c.customer_id, c.first_name, o.order_id
FROM
    orders o
        RIGHT JOIN
    customers c ON c.customer_id = o.customer_id
ORDER BY c.customer_id;
```

## Exercise

```sql
USE sql_store;

SELECT 
    p.product_id, p.name, oi.quantity
FROM
    products p
        LEFT JOIN
    order_items oi ON p.product_id = oi.product_id
ORDER BY p.product_id;
```