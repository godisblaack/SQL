# 35 Creating a Copy of a Table

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/uj3fdBVawwY?si=Dl4DiigH2Hhesx55"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🧠 1. Introduction

Sometimes we need to **copy data** from one table to another — for example:

* Archiving old records (e.g., `orders` → `orders_archived`).
* Creating a backup or snapshot.
* Duplicating a table for testing or analysis.

Instead of writing multiple `INSERT` statements manually, MySQL provides **powerful shortcuts** for this.

---

## ⚙️ 2. Creating a Table from Another Table

The syntax for copying the structure and data of an existing table:

```sql
CREATE TABLE new_table AS
SELECT * FROM existing_table;
```

### 🧩 Example:

```sql
USE sql_store;

CREATE TABLE orders_archived AS
SELECT * FROM orders;
```

✅ This creates a new table called `orders_archived`
✅ It copies all **data** and **columns** from `orders`
❌ It does **not** copy **constraints**, **primary keys**, or **auto-increment** attributes

---

### 🧱 Resulting Table

| Column      | Data Copied     | Constraint Copied   |
| ----------- | --------------- | ------------------- |
| order_id    | ✅ Values copied | ❌ No auto increment |
| customer_id | ✅               | ❌                   |
| order_date  | ✅               | ❌                   |
| status      | ✅               | ❌                   |

> 🟡 **Important:** Since constraints aren’t copied, if you later insert new data into `orders_archived`, you must explicitly provide values for `order_id`, because it’s no longer auto-incremented.

---

## 🔁 3. Copying a Subset of Data

Instead of copying all rows, you can use a **`SELECT` query with a `WHERE` clause**.

### Example – Copy Orders Before 2019

```sql
INSERT INTO orders_archived
SELECT *
FROM orders
WHERE order_date < '2019-01-01';
```

🟢 This copies **only rows** where the order date is before 2019.

### 💡 Why It Works

* The `SELECT` statement retrieves specific rows.
* The `INSERT INTO` statement places the results into the target table.
* You can filter, join, or transform data during the copy.

---

## 🧩 4. Subqueries in SQL

A **subquery** is a `SELECT` statement **nested inside another statement**, such as `CREATE TABLE`, `INSERT`, `UPDATE`, or even another `SELECT`.

Example:

```sql
CREATE TABLE new_table AS
SELECT * FROM old_table WHERE condition;
```

Here, the inner `SELECT` is the **subquery**.

---

## 🧠 5. Practical Exercise (Mosh Example)

### 🎯 Goal:

Create a copy of the `invoices` table as `invoices_archived`,
but replace the `client_id` with the **client name**,
and only copy invoices that have a **payment**.

---

### ✅ Step 1: Explore the Schema

**Tables Involved:**

* `invoices` — has `client_id`, `invoice_total`, `payment_total`, and date columns.
* `clients` — has `client_id`, `name`, `address`, etc.

---

### ✅ Step 2: Join the Tables

Join `invoices` with `clients` to replace `client_id` → `client name`.

```sql
SELECT 
	i.invoice_id,
    i.number,
	c.name AS client,
    i.invoice_total,
    i.payment_total,
    i.invoice_date,
    i.payment_date,
    i.due_date
FROM 
	invoices i
	JOIN clients c
	ON i.client_id = c.client_id
WHERE
	i.payment_date IS NOT NULL;
```

✅ The `WHERE i.payment_date IS NOT NULL` ensures we only select **paid invoices**.

---

### ✅ Step 3: Use the Query to Create a New Table

Now, wrap this `SELECT` inside a `CREATE TABLE`:

```sql
USE sql_invoicing;

CREATE TABLE invoices_archived AS
SELECT 
	i.invoice_id,
    i.number,
	c.name AS client,
    i.invoice_total,
    i.payment_total,
    i.invoice_date,
    i.payment_date,
    i.due_date
FROM 
	invoices i
	JOIN clients c
	ON i.client_id = c.client_id
WHERE
	i.payment_date IS NOT NULL;
```

✅ Creates a **new table** `invoices_archived`
✅ Copies **only paid invoices**
✅ Replaces **client_id** with **client name**

---

## ⚠️ 6. Notes and Best Practices

| Topic               | Explanation                                                                                                            |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Constraints**     | `CREATE TABLE ... AS` does not copy primary keys, indexes, or constraints. Add them manually if needed.                |
| **Auto Increment**  | Auto-increment properties are not preserved.                                                                           |
| **Existing Tables** | Running `CREATE TABLE` with the same name twice will throw an error. Drop the old one first using `DROP TABLE`.        |
| **Performance**     | `INSERT INTO ... SELECT` is much faster than inserting row by row.                                                     |
| **Use Case**        | Perfect for **data archiving**, **table backups**, **data transformation**, or **ETL (Extract-Transform-Load)** tasks. |

---

## 🧾 7. Summary

| Task                                      | Command                                              |
| ----------------------------------------- | ---------------------------------------------------- |
| Create a copy of a table                  | `CREATE TABLE new AS SELECT * FROM old;`             |
| Copy a filtered subset                    | `INSERT INTO new SELECT * FROM old WHERE condition;` |
| Create copy with joins or transformations | `CREATE TABLE new AS SELECT ... JOIN ... WHERE ...;` |
| Remove all rows before re-inserting       | `TRUNCATE TABLE table_name;`                         |

---

## 🧠 8. Quick Recap

* ✅ `CREATE TABLE ... AS SELECT ...` → creates and fills a new table.
* ✅ `INSERT INTO ... SELECT ...` → copies filtered or transformed data.
* ✅ Subqueries make it easy to combine and reuse queries.
* ⚠️ Constraints, indexes, and auto-increment are not copied automatically.

---

## 9. Queries from the video

```sql
USE sql_store;

CREATE TABLE 
    orders_archived 
    AS
SELECT 
    * 
FROM 
    orders;

INSERT INTO 
    orders_archived
SELECT 
	*
FROM 
	orders
WHERE 
	order_date < '2019-01-01';
```

---

## 10. Exercise

### Create copy of invoices table and put it into  a new table called invoices_archived

```sql
USE sql_invoicing;

CREATE TABLE 
    invoices_archived 
    AS
SELECT 
	i.invoice_id,
    i.number,
	c.name AS client,
    i.invoice_total,
    i.payment_total,
    i.invoice_date,
    i.payment_date,
    i.due_date
FROM 
	invoices i
		JOIN
	clients c
		ON
	i.client_id = c.client_id
WHERE
	i.payment_date IS NOT NULL;
```