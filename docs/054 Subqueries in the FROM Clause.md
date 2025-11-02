# 54 Subqueries in the FROM Clause

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/7AlbT6vAd3o?si=XpiVBlMSZc3AGRn5"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🧠 1. Concept — Subqueries in the `FROM` Clause

A **subquery in the FROM clause** allows you to create a **virtual table** (temporary result set) inside your query.
This virtual table exists **only in memory** while the query runs.

It’s useful when you want to:

* Aggregate or summarize data first,
* Then apply further filtering, joins, or grouping logic on top of that result.

---

## ⚙️ 2. Syntax Overview

```sql
SELECT 
    outer_columns
FROM (
        SELECT inner_columns
        FROM some_table
        WHERE condition
    ) AS alias_name
WHERE 
    filter_condition;
```

### 💡 Key Rule

Every subquery used in the `FROM` clause **must have an alias** — even if you don’t use it.
Otherwise, MySQL will throw an error.

---

## 🧩 3. Example — Sales Summary Report

Let’s use your `sql_invoicing` database example from the lecture.

### 🔹 Step 1: Inner Query (The Virtual Table)

This subquery calculates total sales and averages per client:

```sql
SELECT
    c.client_id,
    c.name,
    (
        SELECT SUM(invoice_total)
        FROM invoices
        WHERE client_id = c.client_id
    ) AS total_sales,
    (
        SELECT AVG(invoice_total)
        FROM invoices
    ) AS average,
    (
        (
            SELECT SUM(invoice_total)
            FROM invoices
            WHERE client_id = c.client_id
        ) -
        (
            SELECT AVG(invoice_total)
            FROM invoices
        )
    ) AS difference
FROM
    clients c;
```

👉 This query creates a **virtual table** like:

| client_id | name        | total_sales | average    | difference |
| --------- | ----------- | ----------- | ---------- | ---------- |
| 1         | Vinte       | 802.89      | 152.388235 | 650.501765 |
| 2         | Myworks     | 101.79      | 152.388235 | -50.598235 |
| 3         | Yadel       | 705.90      | 152.388235 | 553.511765 |
| 4         | Kwideo      | NULL        | 152.388235 | NULL       |
| 5         | Topiclounge | 980.02      | 152.388235 | 827.631765 |

---

### 🔹 Step 2: Use It in the `FROM` Clause

Now we’ll treat that result like a real table:

```sql
SELECT
    *
FROM (
    SELECT
        c.client_id,
        c.name,
        (
            SELECT SUM(invoice_total)
            FROM invoices
            WHERE client_id = c.client_id
        ) AS total_sales,
        (
            SELECT AVG(invoice_total)
            FROM invoices
        ) AS average,
        (
            (
                SELECT SUM(invoice_total)
                FROM invoices
                WHERE client_id = c.client_id
            ) -
            (
                SELECT AVG(invoice_total)
                FROM invoices
            )
        ) AS difference
    FROM
        clients c
) AS sales_summary
WHERE
    total_sales IS NOT NULL;
```

---

## 🔍 4. Explanation

| Step | What Happens                                                                                                                 |
| ---- | ---------------------------------------------------------------------------------------------------------------------------- |
| 1️⃣  | The **inner subquery** runs first, creating a virtual table `sales_summary` in memory.                                       |
| 2️⃣  | The **outer query** then runs on top of that result — filtering out clients with no sales (`WHERE total_sales IS NOT NULL`). |
| 3️⃣  | You can now **filter, sort, or join** `sales_summary` as if it were a real database table.                                   |

---

## ⚙️ 5. Practical Use Cases

| Use Case                              | Example                                                      |
| ------------------------------------- | ------------------------------------------------------------ |
| **Filtering on aggregated data**      | e.g., return only clients with total sales > average         |
| **Joining summaries to other tables** | e.g., join sales summary to region or account manager tables |
| **Simplifying multi-level logic**     | Break complex aggregations into readable steps               |

---

## 💡 6. Why Use Subqueries in `FROM`?

✅ **Advantages**

* Lets you treat computed data as a table.
* Helps break down complex logic into smaller, understandable parts.
* Useful when you need to filter or join on aggregated values.

⚠️ **Disadvantages**

* Can become **hard to read** if overused or deeply nested.
* Sometimes **slower** than using a **CTE** (`WITH` clause) or a **VIEW**.
* Harder to debug or optimize compared to joins.

---

## 🧰 7. Alternative: Using a View (Cleaner, Reusable)

Instead of repeating that large subquery, you can store it as a **view**:

```sql
CREATE VIEW sales_summary AS
SELECT
    c.client_id,
    c.name,
    (
        SELECT SUM(invoice_total)
        FROM invoices
        WHERE client_id = c.client_id
    ) AS total_sales,
    (
        SELECT AVG(invoice_total)
        FROM invoices
    ) AS average,
    (
        (
            SELECT SUM(invoice_total)
            FROM invoices
            WHERE client_id = c.client_id
        ) -
        (
            SELECT AVG(invoice_total)
            FROM invoices
        )
    ) AS difference
FROM
    clients c;
```

Then you can simply query:

```sql
SELECT *
FROM sales_summary
WHERE total_sales IS NOT NULL;
```

✅ **Much cleaner**, reusable, and easier to maintain.

---

## 🧩 8. Key Takeaways

| Concept              | Description                                       |
| -------------------- | ------------------------------------------------- |
| **Subquery in FROM** | Turns a query result into a temporary table       |
| **Alias required**   | Always assign an alias (e.g., `AS sales_summary`) |
| **Used for**         | Aggregation, filtering, joining summarized data   |
| **Performance**      | May run slower than joins or CTEs on large data   |
| **Better option**    | Use a **VIEW** or **CTE** when query gets complex |

---

## 9. Queries from the video

```sql
USE sql_invoicing;

SELECT
	*
FROM (
	SELECT
		c.client_id,
		c.name,
		(
			SELECT
				SUM(invoice_total)
			FROM
				invoices
			WHERE
				client_id = c.client_id
		) AS total_sales,
		(
			SELECT
				AVG(invoice_total)
			FROM
				invoices
		) AS average,
		(SELECT
			total_sales
		) - (
			SELECT
				average
		) AS difference
	FROM
		clients c
) AS sales_summary
WHERE
	total_sales IS NOT NULL;
```