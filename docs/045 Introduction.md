# 45 Introduction

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/Ca8LmZphdyQ?si=iC-3Yf8ga8S_m0yO"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🔍 1. What is a Subquery?

A **subquery** (or *nested query*) is a **`SELECT` statement inside another SQL statement**.

Subqueries allow you to:

* Use the **result of one query** as input for another.
* Perform **comparisons**, **filters**, or **aggregations** that depend on data from another table.
* Write queries that would otherwise require **multiple steps** or **temporary tables**.

---

## 🧩 2. Subquery Examples (Conceptually)

### Example 1 — Subquery in a `WHERE` Clause

```sql
SELECT *
FROM invoices
WHERE client_id = (
    SELECT client_id
    FROM clients
    WHERE name = 'Myworks'
);
```

🟢 **Explanation:**

* The inner query fetches the `client_id` of the client named *Myworks*.
* The outer query uses that ID to find all invoices for that client.

---

### Example 2 — Subquery in a `SELECT` Clause

```sql
SELECT
    first_name,
    last_name,
    (SELECT COUNT(*)
     FROM orders o
     WHERE o.customer_id = c.customer_id) AS total_orders
FROM
    customers c;
```

🟢 **Explanation:**

* For each customer, we calculate the total number of orders they’ve placed using a subquery.

---

### Example 3 — Subquery in a `FROM` Clause

```sql
SELECT
    state,
    AVG(total_sales) AS avg_sales
FROM (
    SELECT
        state,
        SUM(invoice_total) AS total_sales
    FROM
        invoices i
        JOIN clients c USING (client_id)
    GROUP BY state
) AS state_sales
GROUP BY state;
```

🟢 **Explanation:**

* The inner query summarizes sales by state.
* The outer query calculates the **average sales** across those states.

---

## ⚙️ 3. Why Subqueries Are Useful

| Use Case           | Description                                                                |
| ------------------ | -------------------------------------------------------------------------- |
| Filtering          | Use results of one query to filter another (`WHERE ... IN`, `WHERE ... =`) |
| Aggregation        | Compute totals or averages in steps                                        |
| Dynamic Conditions | Pass results from one part of a query to another dynamically               |
| Simplifying Logic  | Avoids the need for temporary tables or multiple joins                     |

---

## 🧾 4. Before You Begin — Restoring Databases

Before starting this section, it’s **essential to restore all the databases** to their **original state**, since earlier exercises involved **adding, updating, or deleting** data.

### 🔧 Steps to Restore Databases in MySQL Workbench

1. **Open MySQL Workbench**
2. On the **top menu**, click **File → Open SQL Script**
3. **Browse** to the folder where you stored the SQL scripts for this course
   (If you lost it, go back to the *first section* where you downloaded supplementary materials.)
4. **Open** the file named:

   ```
   create_databases.sql
   ```
5. **Execute** the script:

   * Click the **Execute (⚡️)** button, or
   * Use the **keyboard shortcut (Ctrl + Shift + Enter)**
6. Wait for confirmation that all databases have been recreated.
7. On the **Navigator panel**, click the **refresh icon** to reload the database list.

✅ **All databases are now reset** to their original state — this ensures your data matches exactly what is used in the upcoming lessons.

---

## 🧠 5. Summary

| Concept              | Description                                            |
| -------------------- | ------------------------------------------------------ |
| **Subquery**         | A `SELECT` query inside another query                  |
| **Purpose**          | Used for filtering, aggregation, and complex logic     |
| **Common Locations** | `SELECT`, `FROM`, and `WHERE` clauses                  |
| **Why Restore Data** | To ensure consistent results with the course exercises |
| **Next Step**        | Begin working with subqueries in various contexts      |
