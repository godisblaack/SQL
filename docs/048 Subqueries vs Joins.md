# 48 Subqueries vs Joins

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/fQn_2bDXY-g?si=o3DSiF5sYA9lUvE2"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🧩 1. Subqueries vs Joins — What’s the Difference?

In SQL, many problems can be solved **either with a subquery or with a join**.
Both approaches can produce the same result — the difference lies mainly in **how** they express relationships and **how readable or efficient** they are.

In this lesson, we’ll explore how to **rewrite subqueries using joins**, understand **when to choose each**, and practice both approaches.

---

## ⚙️ 2. Rewriting Subqueries as Joins

Let’s start with a query we wrote earlier — finding **clients without invoices** in the `sql_invoicing` database.

### 🧩 Subquery Version

```sql
USE sql_invoicing;

SELECT
    *
FROM
    clients
WHERE client_id NOT IN (
    SELECT DISTINCT
        client_id
    FROM
        invoices
);
```

### 🔍 Explanation:

* The inner query returns the list of **client IDs** that appear in the `invoices` table.
* The outer query returns **clients whose IDs are not in that list**, i.e., clients who **have no invoices**.

✅ **Output:**

| client_id | name         | city        |
| --------- | ------------ | ----------- |
| 10        | Modern Foods | Los Angeles |

---

### 🧩 Join Version

We can rewrite the same logic using a **LEFT JOIN** instead of a subquery.

```sql
SELECT
    *
FROM
    clients
        LEFT JOIN
    invoices
        USING (client_id)
WHERE
    invoice_id IS NULL;
```

### 🔍 Explanation:

* The **LEFT JOIN** returns **all clients**, even those without matching invoices.
* For clients **without invoices**, the `invoice_id` field will be `NULL`.
* The `WHERE invoice_id IS NULL` condition filters out clients who **do have** invoices.

✅ **Result:** identical to the subquery version.

---

## 🧠 3. Choosing Between Subquery and Join

| Criteria                  | Subquery                                                       | Join                                                |
| ------------------------- | -------------------------------------------------------------- | --------------------------------------------------- |
| **Conceptual simplicity** | Easy when expressing logic like “find items not in…”           | Better when expressing data relationships           |
| **Performance**           | Sometimes slower (depends on database engine and optimization) | Often faster for large joins, but not always        |
| **Readability**           | Feels more natural for “existence” or “non-existence” checks   | Feels more natural when dealing with related tables |
| **Example**               | Clients **without** invoices                                   | Clients **and** their invoices                      |

💡 **Rule of thumb:**
If your problem sounds like “find items that exist or don’t exist in another list,” use a **subquery**.
If it’s about **combining related tables**, a **join** often feels more intuitive.

---

## 🧩 4. Exercise — Customers Who Ordered Lettuce

Now let’s solve a new problem in the `sql_store` database:
Find all **customers who have ordered lettuce** (where product ID = 3).

We’ll solve it both ways — using a **subquery** and a **join**.

---

### 🧠 Subquery Solution

```sql
USE sql_store;

SELECT
    customer_id,
    first_name,
    last_name
FROM
    customers
WHERE
    customer_id IN (
        SELECT
            o.customer_id
        FROM
            order_items oi
                JOIN
            orders o
                USING (order_id)
        WHERE
            product_id = 3
    );
```

### 🔍 Explanation:

* The **inner query** finds all customers (via `orders`) who purchased **product_id = 3** (Lettuce).
* The **outer query** retrieves those customers’ details from the `customers` table.
* This is clean and intuitive when thinking, “Find customers who are **in the list** of lettuce buyers.”

✅ **Output:**

| customer_id | first_name | last_name |
| ----------- | ---------- | --------- |
| 2           | John       | Smith     |
| 8           | Emma       | Brown     |
| 10          | Michael    | Lee       |

---

### 🧠 Join Solution

```sql
SELECT DISTINCT
    customer_id,
    first_name,
    last_name
FROM
    customers
        JOIN
    orders o
        USING (customer_id)
        JOIN
    order_items oi
        USING (order_id)
WHERE
    oi.product_id = 3;
```

### 🔍 Explanation:

* Here, we **join** the three related tables:

  * `customers` → `orders` → `order_items`
* We filter where the `product_id` equals **3 (Lettuce)**.
* The `DISTINCT` keyword ensures we don’t list customers multiple times if they ordered lettuce more than once.

✅ **Output:** Same as the subquery result.

---

## 🧩 5. Comparing Both Approaches

| Approach     | Pros                                                                    | Cons                                                                 |
| ------------ | ----------------------------------------------------------------------- | -------------------------------------------------------------------- |
| **Subquery** | Intuitive for “who’s in/out of a list”; easy to read for small problems | Can become harder to follow when nested deeply                       |
| **Join**     | Shows relationships clearly; often faster for multi-table lookups       | Slightly more verbose; can be overkill for simple “existence” checks |

💬 **Instructor’s Tip:**
In this example, the **JOIN version** is more readable because the relationship between
`customers → orders → order_items` is explicit and natural.
It describes **how data connects**, not just **who’s included**.

---

## ⚙️ 6. Key Rules and Notes

1. **Subqueries** and **joins** can often replace each other.
2. **LEFT JOIN + IS NULL** can replace a `NOT IN` subquery.
3. Always test both versions when performance matters — behavior may vary by dataset.
4. **Readability > brevity** — prefer the version that clearly communicates intent.
5. Use `DISTINCT` when joins create duplicates.

---

## 💡 7. Key Takeaways

| Concept           | Description                                                                     |
| ----------------- | ------------------------------------------------------------------------------- |
| **Subquery**      | A query inside another query, useful for inclusion/exclusion logic              |
| **Join**          | Combines rows from related tables based on matching columns                     |
| **Equivalence**   | Many subqueries can be rewritten as joins (and vice versa)                      |
| **Performance**   | Depends on table size and indexing                                              |
| **Best practice** | Pick the query style that is **most readable** and **fits the logic naturally** |

---

## 🚀 8. Real-World Use Cases

* Subquery → Find **users who haven’t placed orders**
* Join → Retrieve **orders with customer and product details**
* Subquery → Identify **products not in stock**
* Join → Combine **sales data across multiple tables**
* Hybrid → Mix both for **complex business reports**

---

## 9. Queries from the video

```sql
USE sql_invoicing;

SELECT
    *
FROM
    clients
WHERE client_id NOT IN (
    SELECT DISTINCT
        client_id
    FROM
        invoices
);

SELECT
	*
FROM
	clients
		LEFT JOIN
	inboices
		USING (client_id)
WHERE
	invoice_id IS NULL;
```
## 10. Exercise

### Find customers who have ordered lettuce (id = 3). Select customer_id, first_name, last_name.

```sql
USE sql_store; 

-- My solution
SELECT DISTINCT
	c.customer_id,
    c.first_name,
    c.last_name
FROM
	orders o
		JOIN
	(
	SELECT
		order_id
	FROM
		order_items
	WHERE
		product_id = 3
) oi
		ON
	o.order_id = oi.order_id
		JOIN
	customers c
		ON
	o.customer_id = c.customer_id;

-- Mosh's solution
SELECT
	customer_id,
    first_name,
    last_name
FROM
	customers
WHERE
	customer_id IN (
	SELECT
		o.customer_id
    FROM
		order_items oi
    JOIN
		orders o
			USING 
		(order_id)
    WHERE
		product_id = 3
);

SELECT DISTINCT
	customer_id,
    first_name,
    last_name
FROM
	customers
		JOIN
	orders o
		USING 
	(customer_id)
		JOIN
	order_items oi
		USING 
	(order_id)
WHERE
	oi.product_id = 3;
```