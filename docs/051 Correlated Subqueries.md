# 51 Correlated Subqueries

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/SyMp6YKCS78?si=MHE6DzyTDy_YXQRc"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🧩 1. Overview

In this lesson, we’ll explore **correlated subqueries** — a powerful way to make your SQL queries context-aware.
We’ll find employees whose salary is **above the average** salary **within their own office**.

---

## ⚙️ 2. Problem Breakdown

We’re working with the `sql_hr` database, specifically the `employees` table.

Each employee record includes:

* `employee_id`
* `salary`
* `office_id`

| employee_id | first_name | salary | office_id |
| ----------- | ---------- | ------ | --------- |
| 1           | Sarah      | 60,000 | 1         |
| 2           | John       | 72,000 | 1         |
| 3           | Emma       | 55,000 | 1         |
| 4           | David      | 90,000 | 2         |

We want to:

1. Compute the **average salary per office**, and
2. Return only employees whose salary is **greater than** that average.

---

## ⚙️ 3. Step 1 — The Pseudocode Logic

Here’s how we might describe it in plain English:

```
For each employee:
    Find the average salary in that employee's office
    If employee’s salary > average salary of their office:
        Include this employee
```

---

## ⚙️ 4. Step 2 — Translating to SQL

Let’s turn this logic into a query.

```sql
USE sql_hr;

SELECT
    *
FROM
    employees e
WHERE
    salary > (
        SELECT
            AVG(salary)
        FROM
            employees
        WHERE
            office_id = e.office_id
    );
```

---

## 🔍 5. Understanding the Query

### 🧠 Inner Query

```sql
SELECT AVG(salary)
FROM employees
WHERE office_id = e.office_id;
```

* Calculates the **average salary** for the office that the current employee (`e`) belongs to.
* The reference `e.office_id` comes from the **outer query** — this is the **correlation**.

### 🧩 Outer Query

```sql
SELECT * FROM employees e WHERE salary > (subquery);
```

* For **each row** in `employees`, MySQL executes the subquery.
* If that employee’s `salary` is greater than their office’s average, they are included in the result.

---

## 🔄 6. What Makes It a *Correlated Subquery*?

| Type                      | Description                                                                                             |
| ------------------------- | ------------------------------------------------------------------------------------------------------- |
| **Uncorrelated Subquery** | Runs **once**; result passed to the main query. Example: `salary > (SELECT AVG(salary) FROM employees)` |
| **Correlated Subquery**   | Runs **for every row** in the main query; uses values from that row in its own condition.               |

In this query, the subquery depends on `e.office_id` — hence it’s **correlated**.

---

## 🧩 7. Example Flow Visualization

```
For each employee e in employees:
    → Run subquery:
        SELECT AVG(salary)
        FROM employees
        WHERE office_id = e.office_id;
    → Compare e.salary > (average)
    → If true → include in result
```

✅ Output: Employees earning more than the average salary **in their own office**.

---

## ⚙️ 8. Step 3 — Exercise: Invoices Above Client’s Average

Now let’s apply the same concept to another table — `invoices` in the `sql_invoicing` database.

### 💬 Problem

> Find all invoices that are larger than their client’s **average invoice amount**.

Each client can have multiple invoices.
For each client, we calculate their **average invoice total**, then compare each invoice against it.

---

### ✅ Solution

```sql
USE sql_invoicing;

SELECT 
    *
FROM
    invoices i
WHERE
    invoice_total > (
        SELECT
            AVG(invoice_total)
        FROM
            invoices
        WHERE
            client_id = i.client_id
    );
```

---

## 🔍 9. Explanation

| Part            | Description                                                                                        |
| --------------- | -------------------------------------------------------------------------------------------------- |
| **Inner Query** | Calculates average invoice total **per client**. It references `i.client_id` from the outer query. |
| **Outer Query** | Compares each invoice’s `invoice_total` with that average.                                         |
| **Result**      | Returns invoices that are **above their client’s own average** invoice amount.                     |

✅ Example:
If Client 5 has invoices `[100, 200, 300]`,
average = 200 → returns only invoices 300 (above average).

---

## 🧠 10. Correlated Subqueries vs Joins

| Feature         | Correlated Subquery                          | Join                            |
| --------------- | -------------------------------------------- | ------------------------------- |
| **Execution**   | Runs subquery for each row                   | Processes all data sets at once |
| **Performance** | Slower for large tables                      | Usually faster                  |
| **Readability** | More intuitive for per-row logic             | Better for set-based logic      |
| **Use Case**    | Per-row comparisons (like averages or ranks) | Combining related tables        |

---

## ⚙️ 11. Performance Consideration

Correlated subqueries are powerful but can be **expensive** for large datasets because:

* The subquery executes once **per row** of the outer query.
* In contrast, uncorrelated subqueries execute **only once**.

**Optimization Tip:**
If performance becomes an issue, consider rewriting using a **join with a derived table**.

---

## 💡 12. Key Takeaways

| Concept                   | Description                                                                           |
| ------------------------- | ------------------------------------------------------------------------------------- |
| **Correlated Subquery**   | A subquery that depends on values from the outer query                                |
| **Uncorrelated Subquery** | Independent subquery; runs once                                                       |
| **Performance**           | Correlated subqueries can be slower on large datasets                                 |
| **Use Case Example**      | “Find items above average per category,” “Find invoices above client’s average,” etc. |
| **Alias Importance**      | Always alias tables to clearly differentiate outer vs inner references                |

---

## 🧩 13. Summary Visualization

```
Outer Query (employees e)
│
├─ Subquery: AVG(salary)
│   └─ WHERE office_id = e.office_id
│
└─ Compare e.salary > (subquery result)
```

✅ Result → Employees earning above their office average.
✅ Same pattern applies to → Invoices above client average.

---

## 14. Queries from the video

```sql
USE sql_hr;

SELECT
    *
FROM
	employees e
WHERE
	salary > (
		SELECT
			AVG(salary)
		FROM
			employees
		WHERE
			office_id = e.office_id
	);

-- Query from the last video

USE sql_invoicing;

SELECT
	*
FROM
	clients
WHERE
	client_id IN (
		SELECT
			client_id
		FROM
			invoices
		GROUP BY
			client_id
		HAVING 
			COUNT(*) >= 2
    );
```

## 15. Exercise

### Get invoices that are larger than the client's average invoice amount

```sql
USE sql_invoicing;

SELECT 
	*
FROM
	invoices i
WHERE
	invoice_total > (
		SELECT
            AVG(invoice_total)
		FROM
			invoices
		WHERE
			client_id = i.client_id
	);
```