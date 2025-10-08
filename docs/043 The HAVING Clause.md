# 43 The HAVING Clause

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/cQhzXpxGY-8?si=ylAKdhrEm7a52tRy"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🎯 1. Overview

In the previous lesson, you learned how to **summarize and group data** using the `GROUP BY` clause and aggregate functions such as `SUM()` and `COUNT()`.

Now, we’ll learn how to **filter grouped results** using the **`HAVING`** clause — an essential tool when working with aggregate data.

---

## 🔹 2. The Problem with `WHERE` in Aggregates

Let’s recall our earlier example — calculating **total sales per client**:

```sql
USE sql_invoicing;

SELECT 
    client_id,
    SUM(invoice_total) AS total_sales
FROM
    invoices
GROUP BY 
	client_id;
```

✅ Output:

| client_id | total_sales |
| --------- | ----------- |
| 1         | 300.00      |
| 2         | 400.00      |
| 3         | 1100.00     |
| 4         | 900.00      |

Now suppose we only want clients whose **total sales exceed $500**.

You might be tempted to write:

```sql
WHERE total_sales > 500;
```

But that will fail ❌ — because **`WHERE` filters rows before aggregation**, and at that stage, `total_sales` doesn’t exist yet!

---

## ⚙️ 3. Solution — The `HAVING` Clause

We use **`HAVING`** to filter results **after grouping and aggregation**.

```sql
SELECT 
    client_id,
    SUM(invoice_total) AS total_sales
FROM
    invoices
GROUP BY 
	client_id
HAVING 
	total_sales > 500;
```

✅ Output:

| client_id | total_sales |
| --------- | ----------- |
| 3         | 1100.00     |
| 4         | 900.00      |

👉 The `HAVING` clause filters out all groups that do not meet the condition.

---

## 🔍 4. Difference Between `WHERE` and `HAVING`

| Clause     | Filters When    | Can Use Aggregate Functions? | Typical Usage          |
| ---------- | --------------- | ---------------------------- | ---------------------- |
| **WHERE**  | Before grouping | ❌ No                         | Filter raw rows        |
| **HAVING** | After grouping  | ✅ Yes                        | Filter grouped results |

### Example Comparison

```sql
-- Filter rows before grouping
WHERE client_id = 3

-- Filter grouped results after SUM()
HAVING SUM(invoice_total) > 500
```

---

## 🧮 5. Adding More Aggregates

Let’s include more columns, such as **number of invoices per client**, along with their total sales:

```sql
SELECT 
    client_id, 
    SUM(invoice_total) AS total_sales,
    COUNT(*) AS number_of_invoices
FROM
    invoices
GROUP BY 
	client_id;
```

✅ Output:

| client_id | total_sales | number_of_invoices |
| --------- | ----------- | ------------------ |
| 1         | 300.00      | 2                  |
| 2         | 400.00      | 1                  |
| 3         | 1100.00     | 7                  |
| 4         | 900.00      | 8                  |

---

## 🔎 6. Filtering Groups with Multiple Conditions

Let’s now return only clients who have:

* **total sales > $500**, **AND**
* **more than 5 invoices**.

```sql
SELECT 
    client_id, 
    SUM(invoice_total) AS total_sales,
    COUNT(*) AS number_of_invoices
FROM
    invoices
GROUP BY 
	client_id
HAVING 
	total_sales > 500
	AND
	number_of_invoices > 5;
```

✅ Output:

| client_id | total_sales | number_of_invoices |
| --------- | ----------- | ------------------ |
| 3         | 1100.00     | 7                  |
| 4         | 900.00      | 8                  |

Beautiful!
Now we see only clients who meet both conditions.

---

## 🧠 7. Rule of Thumb

* `WHERE` → used **before** grouping (filters rows).
* `HAVING` → used **after** grouping (filters groups).
* In `HAVING`, you can reference **aggregate functions** or their **aliases**.
* In `WHERE`, you can only reference **columns** that exist in the table.

---

## 💡 8. Example: Realistic Business Query

Now let’s do a more complex **exercise** combining joins, grouping, and `HAVING`.

---

## 🧩 9. Exercise — Customers from Virginia Who Spent Over $100

### 🧠 Problem Breakdown

We need customers who:

1. Live in **Virginia (`state = 'VA'`)**.
2. Have **total purchases > $100**.

Let’s figure out where this information resides:

* Customer info → `customers` table.
* Order info → `orders` table.
* Order details (price, quantity) → `order_items` table.

We need to **join these three tables** to get complete data.

---

### ✅ Step 1: Filter Virginia Customers

```sql
USE sql_store;

SELECT *
FROM customers
WHERE state = 'VA';
```

✅ Output:
Two customers from Virginia.

---

### ✅ Step 2: Join with Orders and Order Items

We need total spending per customer — that requires joining all related tables.

```sql
SELECT *
FROM customers c
JOIN orders o
	ON c.customer_id = o.customer_id
JOIN order_items oi
	ON o.order_id = oi.order_id;
```

This returns customer + order + item details — too many columns, but we’ll refine it.

---

### ✅ Step 3: Calculate Total Sales per Customer

Now, let’s compute the total each customer spent using an **aggregate function**.

```sql
SELECT
    c.customer_id,
    c.first_name,
    c.last_name,
    SUM(oi.quantity * oi.unit_price) AS total_sales
FROM
	customers c
		JOIN
	orders o
		ON c.customer_id = o.customer_id
		JOIN
	order_items oi
		ON o.order_id = oi.order_id
WHERE
	 state = 'VA'
GROUP BY
	c.customer_id,
	c.first_name,
	c.last_name;
```

✅ Output:

| customer_id | first_name | last_name | total_sales |
| ----------- | ---------- | --------- | ----------- |
| 1           | Olivia     | Smith     | 85.00       |
| 2           | Ethan      | Johnson   | 157.00      |

---

### ✅ Step 4: Apply `HAVING` Filter on Total Sales

Now we want only customers whose **total sales > 100**.

```sql
SELECT
    c.customer_id,
    c.first_name,
    c.last_name,
    SUM(oi.quantity * oi.unit_price) AS total_sales
FROM
	customers c
		JOIN
	orders o
		ON c.customer_id = o.customer_id
		JOIN
	order_items oi
		ON o.order_id = oi.order_id
WHERE
	state = 'VA'
GROUP BY
	c.customer_id,
	c.first_name,
	c.last_name
HAVING
	total_sales > 100;
```

✅ Final Output:

| customer_id | first_name | last_name | total_sales |
| ----------- | ---------- | --------- | ----------- |
| 2           | Ethan      | Johnson   | 157.00      |

---

### 🧠 Step-by-Step Recap

| Step | Description                 | SQL Tool Used                |
| ---- | --------------------------- | ---------------------------- |
| 1️⃣  | Get customers in Virginia   | `WHERE state = 'VA'`         |
| 2️⃣  | Join orders and order_items | `JOIN`                       |
| 3️⃣  | Calculate total sales       | `SUM(quantity * unit_price)` |
| 4️⃣  | Group by customer           | `GROUP BY`                   |
| 5️⃣  | Filter by total sales       | `HAVING total_sales > 100`   |

---

## ⚡ 10. Quick Summary — `WHERE` vs `HAVING`

| Aspect              | `WHERE`              | `HAVING`                        |
| ------------------- | -------------------- | ------------------------------- |
| Execution stage     | Before grouping      | After grouping                  |
| Used with           | Raw columns          | Aggregate results               |
| Can use aggregates? | ❌ No                 | ✅ Yes                           |
| Example             | `WHERE state = 'VA'` | `HAVING SUM(total_sales) > 100` |
| Purpose             | Filter rows          | Filter aggregated data          |

---

## 🧾 11. Final Queries Recap

### 1️⃣ Filter groups by total sales

```sql
SELECT 
    client_id, 
    SUM(invoice_total) AS total_sales
FROM
    invoices
GROUP BY 
	client_id
HAVING 
	total_sales > 500;
```

### 2️⃣ Filter by multiple aggregate conditions

```sql
SELECT 
    client_id, 
    SUM(invoice_total) AS total_sales,
    COUNT(*) AS number_of_invoices
FROM
    invoices
GROUP BY 
	client_id
HAVING 
	total_sales > 500
	AND
	number_of_invoices > 5;
```

### 3️⃣ Exercise — Virginia customers who spent more than $100

```sql
USE sql_store; 

SELECT
    c.customer_id,
    c.first_name,
    c.last_name,
    SUM(oi.quantity * oi.unit_price) AS total_sales
FROM
	customers c
		JOIN
	orders o
		ON c.customer_id = o.customer_id
		JOIN
	order_items oi
		ON o.order_id = oi.order_id
WHERE
	 state = 'VA'
GROUP BY
	c.customer_id,
	c.first_name,
	c.last_name
HAVING
	total_sales > 100;
```

---

## 🧠 12. Key Takeaways

* `HAVING` is like a **WHERE for groups**.
* You can use it with **aggregate functions** such as `SUM()`, `COUNT()`, `AVG()`, etc.
* Clause order still matters:

  ```
  SELECT → FROM → WHERE → GROUP BY → HAVING → ORDER BY
  ```
* You must **`GROUP BY` all non-aggregated columns** in your `SELECT`.
* Combine `WHERE` (to filter raw data) and `HAVING` (to filter results) for maximum control.

---

## 13. Queries from the video

```sql
USE sql_invoicing;

SELECT 
    client_id, 
    SUM(invoice_total) AS total_sales
FROM
    invoices
GROUP BY 
	client_id
HAVING 
	total_sales > 500;
    
SELECT 
    client_id, 
    SUM(invoice_total) AS total_sales,
    COUNT(*) AS number_of_invoices
FROM
    invoices
GROUP BY 
	client_id
HAVING 
	total_sales > 500
		AND
	number_of_invoices > 5;
```

## 14. Exercise

Get the customers who are located in Virginia and who have spent more than $100

```sql
USE sql_store; 

SELECT
    c.customer_id,
    c.first_name,
    c.last_name,
    SUM(oi.quantity * oi.unit_price) AS total_sales
FROM
	customers c
		JOIN
	orders o
		ON
	c.customer_id = o.customer_id
		JOIN
	order_items oi
		ON
	o.order_id = oi.order_id
WHERE
	 state = 'VA'
GROUP BY
	c.customer_id
HAVING
	total_sales > 100;
```