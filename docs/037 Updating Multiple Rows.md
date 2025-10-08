# 37 Updating Multiple Rows

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/ONXUJ7F3scQ?si=lgDKFE29agco2g3Q"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🧠 1. Introduction

In the previous tutorial, we learned how to use the `UPDATE` statement to modify a **single record** in a table.

However, in many real-world applications, you’ll often need to **update multiple records** at once — for example:

* Updating the prices of multiple products,
* Increasing points for a group of customers,
* Marking a batch of invoices as paid.

Fortunately, the **syntax is the same** — the difference lies in **how general your `WHERE` condition** is.

---

## ⚙️ 2. Syntax Recap

```sql
UPDATE table_name
SET column1 = value1,
    column2 = value2,
    ...
WHERE condition;
```

### 🧩 Key Notes:

* The **`WHERE` clause** determines *which rows* are updated.
* If you make the condition **broad**, multiple records may match.
* If you **omit** the `WHERE` clause entirely, **every row** in the table will be updated (⚠️ dangerous).

---

## 📊 3. Example – Updating Multiple Invoices

Let’s look at the **`invoices`** table in the `sql_invoicing` database.

We want to update **all invoices belonging to client number 3**, setting:

* `payment_total` = 50% of `invoice_total`
* `payment_date` = same as `due_date`

```sql
USE sql_invoicing;

UPDATE 
	invoices
SET 
	payment_total = invoice_total * 0.5,
    payment_date = due_date
WHERE
	client_id = 3;
```

✅ This updates **every invoice** with `client_id = 3`
✅ Both columns (`payment_total`, `payment_date`) are updated in a single query
✅ Uses **expressions** to dynamically calculate new values

---

## ⚠️ 4. Safe Update Mode in MySQL Workbench

By default, **MySQL Workbench** runs in *Safe Update Mode*.
This setting prevents accidental mass updates or deletions.

### What It Does:

* It **blocks updates** that don’t use a **primary key** or **LIMIT** clause.
* Even if your `WHERE` clause matches multiple rows, MySQL Workbench may stop the execution to prevent data loss.

### 💡 To Disable Safe Update Mode:

1. Go to the **MySQL Workbench** menu → **Preferences**
2. Click **SQL Editor** on the left sidebar
3. Scroll down and **uncheck** ✅ *“Safe Updates”*
4. Click **OK**
5. **Reconnect** to your database connection
6. Re-run your `UPDATE` statement

> 🔸 This restriction is **specific to MySQL Workbench** — other clients or application code don’t enforce it.

---

## 🧩 5. Updating Multiple Clients Using `IN` Operator

You can also use **logical operators** and **`IN` lists** to target multiple sets of data.

Example:

```sql
UPDATE 
	invoices
SET 
	payment_total = invoice_total * 0.5,
    payment_date = due_date
WHERE
	client_id IN (3, 4);
```

✅ Updates all invoices for **client 3** *and* **client 4**
✅ The `IN` operator simplifies multiple equality checks
(equivalent to `WHERE client_id = 3 OR client_id = 4`)

---

## ⚙️ 6. Updating All Records (No WHERE Clause)

The `WHERE` clause in an `UPDATE` statement is **optional**.

If you omit it, the query updates **every row** in the table.

```sql
UPDATE invoices
SET payment_total = 0;
```

⚠️ **Be very careful** — this can overwrite data across the entire table.

> ✅ Best Practice: Always test with a `SELECT` first to confirm which records will be affected.

---

## 🧮 7. Exercise – Adding Points to Older Customers

### 🧾 Problem:

In the **`sql_store`** database, give **50 bonus points** to all customers born **before 1990**.

### ✅ Solution:

```sql
USE sql_store;

UPDATE
	customers
SET
	points = points + 50
WHERE
	birth_date < '1990-01-01';
```

### 🧩 Explanation:

| Clause                            | Description                                      |
| --------------------------------- | ------------------------------------------------ |
| `UPDATE customers`                | We’re modifying records in the `customers` table |
| `SET points = points + 50`        | Increases current `points` by 50                 |
| `WHERE birth_date < '1990-01-01'` | Filters customers born before Jan 1, 1990        |

✅ Updates only the qualifying records
✅ Uses an **arithmetic expression** in the `SET` clause
✅ Keeps all other customers’ data unchanged

---

## 🧠 8. Combining Update Logic with Conditions

You can use **any condition or operator** from the `WHERE` clause in `UPDATE` statements:

| Operator / Clause         | Example                                              | Description              |
| ------------------------- | ---------------------------------------------------- | ------------------------ |
| `=`                       | `client_id = 3`                                      | Matches specific value   |
| `IN`                      | `client_id IN (3, 4)`                                | Matches a list of values |
| `BETWEEN`                 | `invoice_date BETWEEN '2020-01-01' AND '2020-12-31'` | Matches a date range     |
| `LIKE`                    | `state LIKE 'C%'`                                    | Pattern match            |
| `IS NULL` / `IS NOT NULL` | `payment_date IS NULL`                               | Check for null values    |
| `<`, `>`, `<=`, `>=`      | `points > 1000`                                      | Numeric comparisons      |

---

## 🧾 9. Summary Table

| Concept              | Description                                                  |
| -------------------- | ------------------------------------------------------------ |
| **`UPDATE`**         | Command to modify data                                       |
| **`SET`**            | Defines which columns to update                              |
| **`WHERE`**          | Filters which rows are affected                              |
| **`IN`**             | Matches multiple specific values                             |
| **Safe Update Mode** | Prevents accidental mass updates in MySQL Workbench          |
| **Expressions**      | Allow calculations in updates (e.g., `points = points + 50`) |

---

## ✅ 10. Quick Recap

* 🔹 The **syntax** for updating multiple records is the same as a single record.
* 🔹 The **condition** (`WHERE`) determines how many rows are affected.
* 🔹 Use **`IN`**, **`BETWEEN`**, or **logical operators** for flexible updates.
* 🔹 Be cautious with **Safe Update Mode** — disable it if necessary.
* 🔹 Always confirm with a **`SELECT` query** before applying the update.

---

## 💡 11. Example Flow Summary

| Example                          | Query                             | Affected Rows                  |
| -------------------------------- | --------------------------------- | ------------------------------ |
| Update single client             | `WHERE client_id = 3`             | All invoices for client 3      |
| Update multiple clients          | `WHERE client_id IN (3,4)`        | All invoices for clients 3 & 4 |
| Update all customers before 1990 | `WHERE birth_date < '1990-01-01'` | All older customers            |
| Update all rows (no WHERE)       | (no condition)                    | Entire table (⚠️ risky)        |

---

## 🧩 12. Practice Tip

Before executing a large update, **test it like this**:

```sql
SELECT * FROM customers
WHERE birth_date < '1990-01-01';
```

Then once you’re confident the correct rows are being selected, **apply** the same condition in your `UPDATE` query.

---

## 13. Queries from the video

```sql
USE sql_invoicing;

UPDATE 
	invoices
SET 
	payment_total = invoice_total * 0.5,
    payment_date = due_date
WHERE
	client_id = 3;

UPDATE 
	invoices
SET 
	payment_total = invoice_total * 0.5,
    payment_date = due_date
WHERE -- Optional clause, without this update will reflect on all the column
	client_id IN (3, 4);
```

---

## 14. Exercise

### Write a SQL statement to give any customers born before 1990, 50 extra points

```sql
USE sql_store;

UPDATE
	customers
SET
	points = points + 50
WHERE
	birth_date < '1990-01-01';
```