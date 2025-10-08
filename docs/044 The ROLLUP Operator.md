# 44 The ROLLUP Operator

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/FvulKEu0kK0?si=BImODTwFBkAbp5Z7"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🎯 1. Overview

In the previous lessons, we learned to use:

* Aggregate functions (`SUM`, `AVG`, `COUNT`, etc.)
* `GROUP BY` to summarize data per group
* `HAVING` to filter grouped results

Now, we’ll go one step further and learn how to **generate subtotals and grand totals** automatically using MySQL’s **`WITH ROLLUP`** operator.

---

## 🧮 2. What is `WITH ROLLUP`?

`WITH ROLLUP` is a **MySQL-specific extension** to the `GROUP BY` clause that automatically adds **summary rows** (subtotals and grand totals) to your query results.

It is **not part of standard SQL**, but it’s incredibly useful for reporting and business analytics.

---

## ⚙️ 3. Basic Syntax

```sql
SELECT
    column1,
    AGGREGATE_FUNCTION(column2)
FROM
    table_name
GROUP BY
    column1
WITH ROLLUP;
```

👉 The `WITH ROLLUP` keyword is placed **after the `GROUP BY` clause**.

---

## 🧩 4. Example 1 — Total Sales per Client (with Rollup)

Let’s start with our familiar example:

```sql
USE sql_invoicing;

SELECT 
    client_id, 
    SUM(invoice_total) AS total_sales
FROM
    invoices
GROUP BY 
	client_id
	WITH ROLLUP;
```

✅ Output:

| client_id | total_sales |
| --------- | ----------- |
| 1         | 400.00      |
| 2         | 600.00      |
| 3         | 900.00      |
| 4         | 690.60      |
| **NULL**  | **2590.60** |

### 🔍 What Happened?

* MySQL calculated **total sales for each client**.
* Then, using `WITH ROLLUP`, it **added one extra row** at the bottom.
* This **extra row** contains the **grand total** across all clients.
* The `client_id` is `NULL` because there’s no single client associated with that total.

---

## 🧠 5. Understanding How `ROLLUP` Works

`ROLLUP` builds **hierarchical summaries** based on the columns in your `GROUP BY` list.

If you group by multiple columns, MySQL generates **subtotals for each grouping level**.

---

## 🧩 6. Example 2 — Grouping by Multiple Columns

Let’s group sales by **state** and **city**.

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
	state,
    city
	WITH ROLLUP;
```

✅ Output:

| state    | city          | total_sales |
| -------- | ------------- | ----------- |
| CA       | San Francisco | 800.00      |
| CA       | **NULL**      | **800.00**  |
| NY       | New York      | 600.00      |
| NY       | **NULL**      | **600.00**  |
| OR       | Portland      | 1190.60     |
| OR       | **NULL**      | **1190.60** |
| **NULL** | **NULL**      | **2590.60** |

### 🔍 Explanation:

* For each **state + city** pair → subtotal by city
* For each **state** → subtotal for that state
* At the end → grand total (both columns `NULL`)

💡 **Order of grouping matters!**
MySQL calculates rollups **from right to left** in the `GROUP BY` list.

So, in `GROUP BY state, city`, it:

1. Groups by `city` first,
2. Then summarizes per `state`,
3. Then produces an overall total.

---

## 🧱 7. Structure of Grouped Output

| Grouping Level | Columns with Data | Columns with NULL  | Meaning            |
| -------------- | ----------------- | ------------------ | ------------------ |
| 1              | state, city       | —                  | Subtotal per city  |
| 2              | state             | city = NULL        | Subtotal per state |
| 3              | NULL              | state, city = NULL | Grand total        |

---

## ⚠️ 8. Important Notes About `ROLLUP`

1. `ROLLUP` works **only with aggregate functions** like `SUM`, `COUNT`, `AVG`, etc.
2. The **NULL values** in rollup rows are placeholders for **summary levels**.
3. You **cannot use column aliases** (like `AS total_sales`) inside the `GROUP BY` clause — use actual column names instead.
4. The operator is **MySQL-specific** — it may not work in SQL Server, PostgreSQL, or Oracle (though they have their own equivalents).

---

## 🧩 9. Exercise — Payment Totals by Method (with Rollup)

### 🎯 Goal

Generate a report showing:

* The total payments received **per payment method**, and
* A **grand total** across all methods.

Expected output:

| payment_method | total      |
| -------------- | ---------- |
| Cash           | 10.00      |
| Credit Card    | 351.38     |
| **NULL**       | **361.38** |

---

### 🧠 Step-by-Step Solution

#### **Step 1:** Start with the `payments` table

```sql
SELECT * 
FROM payments;
```

You’ll see columns like:

* `payment_id`
* `date`
* `client_id`
* `payment_method`
* `amount`

We’re interested in **`payment_method`** and **`amount`**.

---

#### **Step 2:** Group by payment method and sum the amounts

```sql
SELECT
    payment_method,
    SUM(amount) AS total
FROM
    payments
GROUP BY
    payment_method;
```

✅ Output (payment method IDs shown):

| payment_method | total  |
| -------------- | ------ |
| 1              | 10.00  |
| 2              | 351.38 |

---

#### **Step 3:** Add `WITH ROLLUP`

```sql
SELECT
    payment_method,
    SUM(amount) AS total
FROM
    payments
GROUP BY
    payment_method
	WITH ROLLUP;
```

✅ Output:

| payment_method | total      |
| -------------- | ---------- |
| 1              | 10.00      |
| 2              | 351.38     |
| **NULL**       | **361.38** |

---

#### **Step 4:** Join with `payment_methods` table

Since `payment_method` stores numeric IDs, we’ll join it with the `payment_methods` table to show human-readable names.

```sql
SELECT
    pm.name AS payment_method,
    SUM(p.amount) AS total
FROM
	payments p
		JOIN
	payment_methods pm
		ON p.payment_method = pm.payment_method_id
GROUP BY
	pm.name
	WITH ROLLUP;
```

✅ Final Output:

| payment_method | total      |
| -------------- | ---------- |
| Cash           | 10.00      |
| Credit Card    | 351.38     |
| **NULL**       | **361.38** |

---

### ⚙️ How It Works

* Each row shows the **total payments received per method**.
* The last row (where `payment_method = NULL`) shows the **grand total**.
* We used `WITH ROLLUP` after grouping to calculate this automatically.

---

## 🧠 10. Key Takeaways

| Concept           | Description                                                          |
| ----------------- | -------------------------------------------------------------------- |
| `WITH ROLLUP`     | Adds subtotals and grand totals to grouped results                   |
| `NULL` in results | Represents a rollup subtotal or grand total                          |
| Works with        | Aggregate functions like `SUM`, `COUNT`, `AVG`                       |
| Clause order      | `SELECT → FROM → WHERE → GROUP BY → WITH ROLLUP → HAVING → ORDER BY` |
| Limitation        | MySQL-specific (not part of ANSI SQL)                                |
| Alias restriction | Cannot use alias in `GROUP BY` when using `ROLLUP`                   |

---

## 💡 11. Real-World Use Cases

* Financial reports showing **sales per region + total sales**
* Inventory reports showing **stock per category + total inventory**
* Payment summaries by **method + total received**
* Performance dashboards with **department subtotals + company-wide total**

---

## 12. Queries from the video

```sql
USE sql_invoicing;

SELECT 
    client_id, 
    SUM(invoice_total) AS total_sales
FROM
    invoices
GROUP BY 
	client_id
		WITH ROLLUP;
        
SELECT 
    state,
    city,
    SUM(invoice_total) AS total_sales
FROM
    invoices i
		JOIN
	clients c
		USING
	(client_id)
GROUP BY 
	state,
    city
		WITH ROLLUP;
```

## 13. Exercise

Generate the following result

| payment_method | total      |
| -------------- | ---------- |
| Cash           | 10.00      |
| Credit Card    | 351.38     |
| **NULL**       | **361.38** |

```sql
USE sql_invoicing; 

SELECT
    pm.name AS payment_method,
    SUM(p.amount) AS total
FROM
	payments p
		JOIN
	payment_methods pm
		ON
	p.payment_method = pm.payment_method_id
GROUP BY
	pm.name
		WITH ROLLUP;
```