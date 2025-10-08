# 42 The GROUP BY Clause

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/zyyBZknjgGo?si=7lnlHH9qq5dmko5e"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🧩 1. What Is `GROUP BY`?

The `GROUP BY` clause groups rows that have the same values in specified columns, so you can apply **aggregate functions** on each group individually.

### 🔹 Basic Syntax

```sql
SELECT 
    column_name,
    AGGREGATE_FUNCTION(column_name)
FROM 
    table_name
[WHERE condition]
GROUP BY 
    column_name;
```

---

## 🧮 2. Example 1 — Total Sales Per Client

Let’s start with a simple aggregation:

```sql
USE sql_invoicing;

SELECT 
    SUM(invoice_total) AS total_sales
FROM
    invoices;
```

✅ Output:
Returns a single total value for all sales — **$2,500.60** (for example).

But what if you want to see **total sales for each client**?

---

### 💡 Grouping by One Column

```sql
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
| 1         | 147.25      |
| 2         | 350.10      |
| 3         | 1120.50     |
| 4         | 870.40      |

Now, each client’s total sales are calculated separately.

---

### ⚙️ Adding Sorting (`ORDER BY`)

By default, MySQL sorts grouped data **by the `GROUP BY` column**.
But you can override that:

```sql
SELECT 
    client_id,
    SUM(invoice_total) AS total_sales
FROM
    invoices
GROUP BY 
    client_id
ORDER BY 
    total_sales DESC;
```

✅ Output (Descending order of total sales):

| client_id | total_sales |
| --------- | ----------- |
| 3         | 1120.50     |
| 4         | 870.40      |
| 2         | 350.10      |
| 1         | 147.25      |

---

## 🔍 3. Filtering Before Grouping (`WHERE` Clause)

We can filter records *before* grouping them.

Example — total sales per client for **the second half of 2019**:

```sql
SELECT 
    client_id,
    SUM(invoice_total) AS total_sales
FROM
    invoices
WHERE
    invoice_date >= '2019-07-01'
GROUP BY 
    client_id
ORDER BY
    total_sales DESC;
```

✅ Output shows sales per client for invoices from **July–December 2019** only.

---

## 🧠 4. Order of SQL Clauses

SQL must follow a specific clause order:

| Order | Clause     | Purpose                     |
| ----- | ---------- | --------------------------- |
| 1️⃣   | `SELECT`   | Choose columns to show      |
| 2️⃣   | `FROM`     | Choose tables               |
| 3️⃣   | `WHERE`    | Filter rows before grouping |
| 4️⃣   | `GROUP BY` | Group rows by column(s)     |
| 5️⃣   | `ORDER BY` | Sort final results          |

> ⚠️ If you place `GROUP BY` after `ORDER BY`, MySQL will throw a **syntax error**.

---

## 🧩 5. Example 2 — Grouping by Multiple Columns

You can group by **two or more columns** to see combinations.

Let’s see **total sales per state and city**:

```sql
SELECT 
    c.state,
    c.city,
    SUM(i.invoice_total) AS total_sales
FROM
    invoices i
        JOIN clients c
        USING (client_id)
GROUP BY 
    c.state,
    c.city;
```

✅ Output:

| state | city        | total_sales |
| ----- | ----------- | ----------- |
| CA    | Los Angeles | 870.40      |
| NY    | New York    | 1120.50     |
| TX    | Dallas      | 350.10      |
| TX    | Austin      | 147.25      |

Each row represents a **unique combination of state and city**.

---

## 🧠 6. Concept Recap

* The **`GROUP BY`** clause divides rows into groups based on column values.
* Aggregate functions (`SUM()`, `COUNT()`, `AVG()`, etc.) are then calculated for each group.
* You can group by **multiple columns** for multi-dimensional summaries.
* Always follow SQL clause order:
  `SELECT` → `FROM` → `WHERE` → `GROUP BY` → `ORDER BY`.

---

## 🧠 7. Tip

When joining tables for grouping:

* Make sure you **join on the correct key columns**.
* Use `USING(column)` when both columns have the same name.
* Use `ON` when column names differ.

---

## 🧮 8. Example 3 — Joining and Grouping by Multiple Columns

We’ll join **payments** and **payment_methods** tables to show **total payments per date and payment method**.

### ✅ Step-by-Step Breakdown

#### Step 1: Group by Date Only

```sql
SELECT
    date,
    SUM(amount) AS total_payments
FROM
    payments
GROUP BY
    date
ORDER BY
    date;
```

✅ Output:

| date       | total_payments |
| ---------- | -------------- |
| 2019-01-03 | 74.55          |
| 2019-01-08 | 42.77          |
| 2019-01-11 | 0.03           |

---

#### Step 2: Add the Payment Method Name (Join Tables)

We need to join the `payments` table with `payment_methods` to get the name.

```sql
SELECT
	p.date AS date,
    pm.name AS payment_method,
    SUM(p.amount) AS total_payments
FROM
	payments p
		JOIN
	payment_methods pm
		ON p.payment_method = pm.payment_method_id
GROUP BY
	p.date,
    p.payment_method
ORDER BY
	p.date;
```

✅ Final Output:

| date       | payment_method | total_payments |
| ---------- | -------------- | -------------- |
| 2019-01-03 | Credit Card    | 74.55          |
| 2019-01-08 | Credit Card    | 32.77          |
| 2019-01-08 | Cash           | 10.00          |
| 2019-01-11 | Credit Card    | 0.03           |
| 2019-01-12 | Credit Card    | 8.18           |
| 2019-01-15 | Credit Card    | 148.41         |
| 2019-01-26 | Credit Card    | 87.44          |

---

### 💡 How It Works

* `GROUP BY p.date, p.payment_method`: creates a group for each **unique (date, payment method)** pair.
* `SUM(p.amount)`: totals all payments within that group.
* The `JOIN` allows us to replace the ID with a readable **payment method name**.

---

## ⚡ 9. Best Practices

✅ **Start simple** — group by one column, then expand to multiple columns.
✅ **Always order results** (`ORDER BY`) to make data easier to read.
✅ **Use aliases** (`AS`) to rename long or computed columns.
✅ **Verify joins** before grouping — incorrect joins lead to inflated totals.
✅ **Filter first** using `WHERE`, not after grouping.

---

## 🧠 10. Key Takeaways

| Concept             | Description                                          |
| ------------------- | ---------------------------------------------------- |
| `GROUP BY`          | Groups rows with same values                         |
| Multiple columns    | Creates groups for every unique combination          |
| Aggregate functions | Perform calculations for each group                  |
| Clause order        | Must appear after `WHERE` and before `ORDER BY`      |
| Joins               | Needed to display readable names from related tables |
| `ORDER BY`          | Sort results after grouping                          |

---

### 🧾 Final Queries Recap

#### 1️⃣ Total Sales Per Client

```sql
USE sql_invoicing;

SELECT 
	client_id,
    SUM(invoice_total) AS total_sales
FROM
    invoices
WHERE
    invoice_date >= '2019-07-01'
GROUP BY 
	client_id
ORDER BY
	total_sales DESC;
```

#### 2️⃣ Total Sales Per State and City

```sql
SELECT 
	state,
    city,
    SUM(invoice_total) AS total_sales
FROM
    invoices i
		JOIN
	clients c
		USING (client_id)
GROUP BY 
	c.state,
    c.city;
```

#### 3️⃣ Total Payments by Date and Method

```sql
SELECT
	p.date AS date,
    pm.name AS payment_method,
    SUM(p.amount) AS total_payments
FROM
	payments p
		JOIN
	payment_methods pm
		ON p.payment_method = pm.payment_method_id
GROUP BY
	p.date,
    p.payment_method
ORDER BY
	p.date;
```

---

## 11. Queries form the video

```sql
USE sql_invoicing;

SELECT 
	client_id,
    SUM(invoice_total) AS total_sales
FROM
    invoices
WHERE
    invoice_date >= '2019-07-01'
GROUP BY 
	client_id
ORDER BY
	total_sales DESC;
    
    
SELECT 
	state,
    city,
    SUM(invoice_total) AS total_sales
FROM
    invoices i
		JOIN
	clients c
		using
	(client_id)
GROUP BY 
	c.client_id;
```

## 12. Exercise

Joining and Grouping by Multiple Columns.

Generate the following result.

| date       | payment_method | total_payments |
| ---------- | -------------- | -------------- |
| 2019-01-03 | Credit Card    | 74.55          |
| 2019-01-08 | Credit Card    | 32.77          |
| 2019-01-08 | Cash           | 10.00          |
| 2019-01-11 | Credit Card    | 0.03           |
| 2019-01-12 | Credit Card    | 8.18           |
| 2019-01-15 | Credit Card    | 148.41         |
| 2019-01-26 | Credit Card    | 87.44          |


```sql
USE sql_invoicing;

SELECT
	p.date AS date,
    pm.name AS payment_method,
    SUM(p.amount) AS total_payments
FROM
	payments p
		JOIN
	payment_methods pm
		ON
	p.payment_method = pm.payment_method_id
GROUP BY
	p.date,
    p.payment_method
ORDER BY
	p.date;
```