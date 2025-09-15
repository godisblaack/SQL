# 27 The USING Clause

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/LEzvLVgdxSA?si=C-KyhyBwmH54RLGo"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Problem with ON Clause

* When joining tables, we typically use the **`ON` clause** to define the join condition.
* Example:

```sql
SELECT 
    o.order_id, c.first_name
FROM orders o
JOIN customers c 
    ON o.customer_id = c.customer_id;
```

* ✅ Works fine, but for queries with multiple joins, these conditions **clutter the query** and make it harder to read.

---

## 2. Simplifying with `USING`

* If the column name is **exactly the same** in both tables, we can replace `ON` with `USING`.
* Example:

```sql
SELECT 
    o.order_id, c.first_name
FROM orders o
JOIN customers c 
    USING (customer_id);
```

* Equivalent to the previous query, but **shorter and more readable**.

---

## 3. Example: Multiple Joins with USING

Joining **orders → customers → shippers**:

```sql
SELECT 
    o.order_id, 
    c.first_name, 
    sh.name AS shipper
FROM orders o
JOIN customers c USING (customer_id)
LEFT JOIN shippers sh USING (shipper_id);
```

### ✅ Notes:

* Inner join between `orders` and `customers` (every order has a customer).
* Left join with `shippers` (not every order has a shipper).
* `USING` works for both inner and outer joins.

---

## 4. Limitation of USING

* `USING` works **only if column names are identical** in both tables.
* Example where it fails:

  * `orders.status` vs `order_statuses.order_status_id`
  * Here we must use `ON` instead of `USING`.

---

## 5. Using with Composite Keys

Sometimes tables use a **composite key** (more than one column).

* Example: `order_items` and `order_item_notes`.

### Without USING:

```sql
SELECT *
FROM order_items oi
JOIN order_item_notes oin 
    ON oi.order_id = oin.order_id
   AND oi.product_id = oin.product_id;
```

### With USING:

```sql
SELECT *
FROM order_items oi
JOIN order_item_notes oin 
    USING (order_id, product_id);
```

* ✅ Much cleaner and easier to read.

---

## 6. Exercise: Payments Report

**Goal:** Show date, client name, amount, and payment method for each payment.

### Query:

```sql
USE sql_invoicing;

SELECT 
    p.date,
    c.name AS client,
    p.amount,
    pm.name AS payment_method
FROM payments p
JOIN clients c USING (client_id)
JOIN payment_methods pm 
    ON p.payment_method = pm.payment_method_id;
```

### ✅ Explanation:

* `payments → clients`: Joined with `USING (client_id)` because both tables share the same column name.
* `payments → payment_methods`: Joined with `ON` because column names differ (`payment_method` vs `payment_method_id`).

---

## 7. Key Takeaways

* ✅ Use `USING` when the column name is **exactly the same** in both tables.
* ✅ Works with both **inner** and **outer** joins.
* ✅ Supports **multiple columns** (composite keys).
* ❌ Cannot be used when column names differ.
* Improves **readability and maintainability** of SQL queries.

---

## Queries from the video

```sql
USE sql_store;

SELECT 
    o.order_id, c.first_name
FROM
    orders o
        JOIN
    customers c USING (customer_id);
    -- ON o.customer_id = c.customer_id
    
SELECT 
    o.order_id, c.first_name
FROM
    orders o
        JOIN
    customers c USING (customer_id)
        JOIN
    shippers sh USING (shipper_id);
    
SELECT 
    o.order_id, c.first_name, sh.name AS shipper
FROM
    orders o
        JOIN
    customers c USING (customer_id)
        LEFT JOIN
    shippers sh USING (shipper_id);
    
SELECT 
    *
FROM
    order_items oi
        JOIN
    order_item_notes oin ON oi.order_id = oin.order_id
        AND oi.product_id = oin.product_id;
        
SELECT 
    *
FROM
    order_items oi
        JOIN
    order_item_notes oin USING (order_id , product_id);
```

## Exercise

```sql
use sql_invoicing;

SELECT 
    p.date, c.name AS client, p.amount, pm.name
FROM
    payments p
        JOIN
    clients c USING (client_id)
        JOIN
    payment_methods pm ON p.payment_method = pm.payment_method_id;

-- Solution by Mosh

SELECT 
    p.date,
    c.name AS client,
    p.amount,
    pm.name AS payment_method
FROM
    payments p
        JOIN
    clients c USING (client_id)
        JOIN
    payment_methods pm ON p.payment_method = pm.payment_method_id;
```