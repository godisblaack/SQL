# 53 Subqueries in the SELECT Clause

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/pffDrBZJFpc?si=VMgDg9H_w8wmArWA"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🧠 1. Concept — Subqueries in the `SELECT` Clause

A **subquery inside the SELECT clause** works like a **calculated field**.
For each row of the outer query, the subquery is executed (if it’s correlated), or once if it’s not.

You can use this to:

* Compute aggregates for each record (like total sales per client)
* Compute overall averages
* Compute comparisons between row-level and global values

---

## ⚙️ 2. Example — Invoice Report (from the lecture)

```sql
USE sql_invoicing;

SELECT
    invoice_id,
    invoice_total,
    (
        SELECT AVG(invoice_total)
        FROM invoices
    ) AS invoice_average,
    invoice_total - (
        SELECT AVG(invoice_total)
        FROM invoices
    ) AS difference
FROM
    invoices;
```

### 🔍 Explanation

* The first subquery computes the **global average** of all invoices.
* The second subquery repeats the same computation to calculate `invoice_total - average`.
* MySQL executes the inner subquery once for each outer row — so it’s identical but a bit redundant.
* We can’t reference `invoice_average` alias inside another expression (it’s not yet available), so we must retype the subquery.

---

## 🧩 3. Exercise — Client Report

We need a report showing:

| client_id | name        | total_sales | average    | difference |
| --------- | ----------- | ----------- | ---------- | ---------- |
| 1         | Vinte       | 802.89      | 152.388235 | 650.501765 |
| 2         | Myworks     | 101.79      | 152.388235 | -50.598235 |
| 3         | Yadel       | 705.90      | 152.388235 | 553.511765 |
| 4         | Kwideo      | NULL        | 152.388235 | NULL       |
| 5         | Topiclounge | 980.02      | 152.388235 | 827.631765 |

---

## ✅ 4. Correct and Optimized Solution

Here’s the **clean, correct query** for that exercise:

```sql
USE sql_invoicing;

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
    (
        (
            SELECT
                SUM(invoice_total)
            FROM
                invoices
            WHERE
                client_id = c.client_id
        )
        -
        (
            SELECT
                AVG(invoice_total)
            FROM
                invoices
        )
    ) AS difference
FROM
    clients c;
```

---

## 🔍 5. Explanation by Column

| Column                  | Description                                                            | Query Type               |
| ----------------------- | ---------------------------------------------------------------------- | ------------------------ |
| `c.client_id`, `c.name` | Data from `clients` table                                              | —                        |
| `total_sales`           | **Correlated subquery** — sum of all invoices for that specific client | Correlated               |
| `average`               | **Uncorrelated subquery** — average of all invoices in the table       | Uncorrelated             |
| `difference`            | Expression comparing `total_sales` vs `average`                        | Combines both subqueries |

### 🧩 Correlated vs Uncorrelated

* **Correlated subquery**: depends on outer query (e.g., `client_id = c.client_id`)
* **Uncorrelated subquery**: independent (e.g., overall average)

---

## ⚙️ 6. Execution Flow

```
For each client (c):
    → Compute SUM(invoice_total) for that client
    → Compute AVG(invoice_total) once for the whole invoices table
    → Subtract to find difference
Return all rows
```

---

## ⚡ 7. Output Example

| client_id | name        | total_sales | average    | difference |
| --------- | ----------- | ----------- | ---------- | ---------- |
| 1         | Vinte       | 802.89      | 152.388235 | 650.501765 |
| 2         | Myworks     | 101.79      | 152.388235 | -50.598235 |
| 3         | Yadel       | 705.90      | 152.388235 | 553.511765 |
| 4         | Kwideo      | NULL        | 152.388235 | NULL       |
| 5         | Topiclounge | 980.02      | 152.388235 | 827.631765 |

---

## 💡 8. Key Takeaways

| Concept                   | Description                                                                                         |
| ------------------------- | --------------------------------------------------------------------------------------------------- |
| **Subquery in SELECT**    | Can return a single value (scalar) used as a calculated field                                       |
| **Correlated subquery**   | Runs once per row — slower but flexible                                                             |
| **Uncorrelated subquery** | Runs once for all rows — faster                                                                     |
| **Alias limitation**      | You cannot reuse column aliases in the same `SELECT` expression                                     |
| **Performance tip**       | For repeated expressions, use CTEs (if supported) or derived tables instead of repeating subqueries |

---

✅ **Bonus Tip (More Efficient Alternative using JOIN + Aggregation)**

If your database supports **CTEs** (MySQL 8+), you can rewrite it for better performance:

```sql
WITH client_sales AS (
    SELECT client_id, SUM(invoice_total) AS total_sales
    FROM invoices
    GROUP BY client_id
),
invoice_stats AS (
    SELECT AVG(invoice_total) AS avg_total FROM invoices
)
SELECT
    c.client_id,
    c.name,
    cs.total_sales,
    i.avg_total AS average,
    cs.total_sales - i.avg_total AS difference
FROM
    clients c
    LEFT JOIN client_sales cs ON c.client_id = cs.client_id
    CROSS JOIN invoice_stats i;
```

🚀 This approach:

* Calculates totals and averages once.
* Avoids correlated subqueries.
* Runs **much faster** on large datasets.

---

## Queries from the video

```sql
USE sql_invoicing;

SELECT
    invoice_id,
    invoice_total,
    (
		SELECT
			AVG(invoice_total)
        FROM
			invoices
	) AS invoice_average,
    invoice_total - (
		SELECT
			AVG(invoice_total)
        FROM
			invoices
	)
FROM
	invoices;
    
SELECT
    invoice_id,
    invoice_total,
    (
		SELECT
			AVG(invoice_total)
        FROM
			invoices
	) AS invoice_average,
    invoice_total - (
		SELECT
			invoice_average
	) AS difference
FROM
	invoices;
```
## Exercise

Return the following result, using sql_invoicing database.

| client_id | name        | total_sales | average    | difference |
| --------- | ----------- | ----------- | ---------- | ---------- |
| 1         | Vinte       | 802.89      | 152.388235 | 650.501765 |
| 2         | Myworks     | 101.79      | 152.388235 | -50.598235 |
| 3         | Yadel       | 705.90      | 152.388235 | 553.511765 |
| 4         | Kwideo      | NULL        | 152.388235 | NULL       |
| 5         | Topiclounge | 980.02      | 152.388235 | 827.631765 |

```sql
USE sql_invoicing;

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
	clients c;
```