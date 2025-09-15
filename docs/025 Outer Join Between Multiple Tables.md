# 25 Outer Join Between Multiple Tables

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/zDDbuzh8Q_8?si=o2i3-rYMp0NMAMJN"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Recap: Left Join with Customers and Orders

We previously used a `LEFT JOIN` to list all customers, regardless of whether they placed orders.

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

✅ Shows all customers (even those without orders).
❌ But orders without shippers are not included yet.

---

## 2. Adding Shippers with an Inner Join (Problem)

Next, we join **orders → shippers**. If we use an **INNER JOIN**, orders without shippers get excluded.

```sql
SELECT 
    c.customer_id, 
    c.first_name, 
    o.order_id, 
    sh.shipper_id
FROM customers c
LEFT JOIN orders o 
    ON c.customer_id = o.customer_id
JOIN shippers sh 
    ON o.shipper_id = sh.shipper_id
ORDER BY c.customer_id;
```

* Only **shipped orders** appear.
* Orders with `NULL` shipper\_id are missing.

---

## 3. Fix: Using Left Join for Shippers

To ensure all orders are shown (even unshipped ones), change the join to a `LEFT JOIN`.

```sql
SELECT 
    c.customer_id, 
    c.first_name, 
    o.order_id, 
    sh.name AS shipper
FROM customers c
LEFT JOIN orders o 
    ON c.customer_id = o.customer_id
LEFT JOIN shippers sh 
    ON o.shipper_id = sh.shipper_id
ORDER BY c.customer_id;
```

✅ Now we see:

* All customers (even without orders).
* All orders (even without shippers → `NULL`).

---

## 4. Best Practice – Avoid RIGHT JOIN

* You can achieve the same results with `RIGHT JOIN` by swapping tables.
* But when combining multiple joins (`INNER`, `LEFT`, `RIGHT`), the query becomes **harder to read**.
* **Recommendation:** Use `LEFT JOIN` consistently to keep logic clear.

---

## 5. Exercise – Orders with Customers, Shippers, and Status

We need a report with:

* Order date
* Order ID
* Customer first name
* Shipper name (nullable)
* Order status

### Step 1 – Start with Orders & Customers

All orders have customers → `INNER JOIN` is fine here.

```sql
SELECT 
    o.order_date,
    o.order_id,
    c.first_name
FROM orders o
JOIN customers c 
    ON o.customer_id = c.customer_id;
```

---

### Step 2 – Add Shippers (Optional)

Not all orders have shippers → use `LEFT JOIN`.

```sql
SELECT 
    o.order_date,
    o.order_id,
    c.first_name,
    s.name AS shipper
FROM orders o
JOIN customers c 
    ON o.customer_id = c.customer_id
LEFT JOIN shippers s 
    ON o.shipper_id = s.shipper_id;
```

---

### Step 3 – Add Order Status

All orders have a status → `INNER JOIN`.

```sql
SELECT 
    o.order_date,
    o.order_id,
    c.first_name,
    s.name AS shipper,
    os.name AS status
FROM orders o
JOIN customers c 
    ON o.customer_id = c.customer_id
LEFT JOIN shippers s 
    ON o.shipper_id = s.shipper_id
JOIN order_statuses os 
    ON o.status = os.order_status_id;
```

---

## 6. Final Solution (by Mosh)

```sql
SELECT 
    o.order_id,
    o.order_date,
    c.first_name AS customer,
    sh.name AS shipper,
    os.name AS status
FROM orders o
JOIN customers c 
    ON o.customer_id = c.customer_id
LEFT JOIN shippers sh 
    ON o.shipper_id = sh.shipper_id
JOIN order_statuses os 
    ON o.status = os.order_status_id;
```

---

## 7. Key Takeaways

* Use `INNER JOIN` where **all records must match** (e.g., every order has a customer).
* Use `LEFT JOIN` where **some records may not match** (e.g., orders without shippers).
* Consistently prefer `LEFT JOIN` over mixing with `RIGHT JOIN` for readability.

---

✨ This pattern (mixing inner and outer joins) is common in real-world reporting queries where some relationships are mandatory, and others are optional.

---

## Queries from the video

```sql
USE sql_store;

SELECT 
    c.customer_id, c.first_name, o.order_id
FROM
    customers c
        LEFT JOIN
    orders o ON c.customer_id = o.customer_id
ORDER BY c.customer_id;

SELECT 
    c.customer_id, c.first_name, o.order_id, sh.shipper_id
FROM
    customers c
        LEFT JOIN
    orders o ON c.customer_id = o.customer_id
        JOIN
    shippers sh ON o.shipper_id = sh.shipper_id
ORDER BY c.customer_id;

SELECT 
    c.customer_id, c.first_name, o.order_id, sh.name AS shipper
FROM
    customers c
        LEFT JOIN
    orders o ON c.customer_id = o.customer_id
        LEFT JOIN
    shippers sh ON o.shipper_id = sh.shipper_id
ORDER BY c.customer_id;
```

## Exercise

```sql
USE sql_store;

SELECT 
    o.order_date,
    o.order_id,
    c.first_name,
    s.name AS shipper,
    os.name AS status
FROM
    orders o
        JOIN
    customers c ON o.customer_id = c.customer_id
        LEFT JOIN
    shippers s ON o.shipper_id = s.shipper_id
        JOIN
    order_statuses os ON o.status = os.order_status_id;
```

### Solution by Mosh

```sql
USE sql_store;

SELECT 
    o.order_id,
    o.order_date,
    c.first_name AS customer,
    sh.name AS shipper,
    os.name AS status
FROM
    orders o
        JOIN
    customers c ON o.customer_id = c.customer_id
        LEFT JOIN
    shippers sh ON o.shipper_id = sh.shipper_id
        JOIN
    order_statuses os ON o.status = os.order_status_id;
```

> NOTE: The order of rows in the result might be different from what is on the video. May be the database has been updated by the Mosh.