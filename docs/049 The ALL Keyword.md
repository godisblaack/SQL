# 49 The ALL Keyword

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/tTVnvl0DQ84?si=H1OdSUfSZ450G1qm"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🧩 1. Overview

In this lesson, we’ll learn how to use **subqueries that return a single value or multiple values**, and how the **`ALL` keyword** can be used to compare against an entire list of results.

---

## ⚙️ 2. Step 1 — Identify the Subquery Target

We want to find **invoices that are larger than all invoices of client 3**.

Let’s start by exploring the data.

```sql
USE sql_invoicing;

SELECT
    *
FROM
    invoices
WHERE
    client_id = 3;
```

### 🔍 Explanation:

This gives us all invoices belonging to **client 3**.
Each invoice has an `invoice_total` amount — for example:

| invoice_id | client_id | invoice_total |
| ---------- | --------- | ------------- |
| 12         | 3         | 130.00        |
| 13         | 3         | 150.00        |
| 15         | 3         | 167.00        |

The **largest invoice** for client 3 is **$167.00**.

---

## ⚙️ 3. Step 2 — Find the Maximum Invoice Total for Client 3

We can now write a query to find this single maximum value:

```sql
SELECT
    MAX(invoice_total)
FROM
    invoices
WHERE
    client_id = 3;
```

✅ **Result:** `167.00`

This query returns **one value** — a perfect candidate for a **scalar subquery**.

---

## ⚙️ 4. Step 3 — Use a Subquery Returning a Single Value

We can now use this value as part of another query.

```sql
SELECT
    *
FROM
    invoices
WHERE
    invoice_total > (
        SELECT
            MAX(invoice_total)
        FROM
            invoices
        WHERE
            client_id = 3
    );
```

### 🔍 Explanation:

* The **inner query** returns the highest total among all invoices for client 3 (`167.00`).
* The **outer query** then finds all invoices in the database whose total is **greater than 167.00**.

✅ **Result Example:**

| invoice_id | client_id | invoice_total |
| ---------- | --------- | ------------- |
| 17         | 2         | 200.00        |
| 19         | 4         | 220.00        |

---

## 🧠 5. Alternative: Using the `ALL` Keyword

There’s another way to express this logic — one that aligns directly with the problem statement.

### 💬 Problem Statement:

> “Find invoices larger than **all invoices** of client 3.”

We can write this naturally in SQL using **`> ALL`**:

```sql
SELECT
    *
FROM
    invoices
WHERE
    invoice_total > ALL (
        SELECT
            invoice_total
        FROM
            invoices
        WHERE
            client_id = 3
    );
```

---

## 🔍 6. How the `ALL` Keyword Works

Let’s break it down conceptually:

1. The **subquery** returns multiple values (one per invoice of client 3), e.g.:

   ```
   130.00, 150.00, 167.00
   ```
2. The **`ALL` keyword** tells MySQL:

   > “Return invoices whose total is greater than **every value** in this list.”

So for each invoice, MySQL checks:

```
Is invoice_total > 130.00 AND > 150.00 AND > 167.00 ?
```

If true, that invoice is included in the result set.

---

## 🔄 7. Comparing Both Approaches

| Approach        | Description                                       | Returns         | Readability                           |
| --------------- | ------------------------------------------------- | --------------- | ------------------------------------- |
| **Using MAX()** | Compares against one single value (the max total) | Scalar value    | Simple, concise                       |
| **Using ALL**   | Compares against a list of all values             | Multiple values | Feels more “natural language” aligned |

Both queries return the **same result**, and both are valid.

---

## 🧩 8. When to Use Each

| Use Case                                                                                           | Recommended Approach                                |
| -------------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| You need to compare against a single number (e.g., highest, lowest, average)                       | Use **aggregate functions** like `MAX()` or `MIN()` |
| You need to compare against every value from a set (logical “greater than all” or “less than all”) | Use the **`ALL` keyword**                           |
| You’re checking “greater than any” or “less than any”                                              | Use the **`ANY`** (or **`SOME`**) keyword           |

---

## ⚙️ 9. Real-World Analogy

Imagine you run a company, and you want to know:

> “Which sales are higher than all sales made by Client 3?”

* If you use the **MAX()** query — you’re comparing to Client 3’s **top sale only**.
* If you use **`> ALL`** — you’re comparing against **every single sale** Client 3 ever made.

Both will yield the same answer in this case (since MAX is the greatest value in that set),
but **`ALL`** expresses the problem **more semantically** — closer to how we describe it in words.

---

## 💡 10. Key Takeaways

| Concept                  | Description                                                                    |
| ------------------------ | ------------------------------------------------------------------------------ |
| **Scalar Subquery**      | Returns a single value (e.g., `MAX()`, `AVG()`)                                |
| **Multi-Value Subquery** | Returns multiple values — must use `IN`, `ANY`, or `ALL`                       |
| **`ALL` Keyword**        | Tests if a value is greater/less than **every value** in a subquery result     |
| **Interchangeability**   | A subquery with `MAX()` and one with `> ALL` can often be used interchangeably |
| **Best Practice**        | Choose the version that’s **most readable and expressive** for your problem    |

---

## 🧠 11. Summary Diagram

```
       invoices table
          ↓
   [Subquery: Client 3]
   └── returns totals → 130, 150, 167
          ↓
  [Main query compares]
  invoice_total > ALL (list above)
          ↓
   Returns invoices with
   totals greater than 167
```

---

## 12. Queries from the video

```sql
USE sql_invoicing;

SELECT
    *
FROM
	invoices
WHERE
	client_id = 3;

SELECT
	MAX(invoice_total)
FROM
	invoices
WHERE
	client_id = 3;
    
SELECT
	*
FROM
	invoices
WHERE
	invoice_total > (
		SELECT
			MAX(invoice_total)
		FROM
			invoices
		WHERE
			client_id = 3
    );
    
SELECT
	*
FROM
	invoices
WHERE
	invoice_total > ALL (
		SELECT
			invoice_total
		FROM
			invoices
		WHERE
			client_id = 3
    );
```