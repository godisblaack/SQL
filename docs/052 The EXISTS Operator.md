# 52 The EXISTS Operator

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/AmD3aSsGF7o?si=BcA7lmUGqbtJmjHW"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🧩 1. Overview

Exploring `EXISTS` operator.

---

## ⚙️ 2. Using the `IN` Operator

### ✅ Query

```sql
USE sql_invoicing;

SELECT
    *
FROM
    clients
WHERE
    client_id IN (
        SELECT DISTINCT
            client_id
        FROM
            invoices
    );
```

### 🔍 Explanation

* The **subquery** gets all `client_id` values from the `invoices` table.
* The **outer query** returns all clients whose `client_id` is in that list.
* The `DISTINCT` keyword removes duplicates (though it’s optional because `IN` inherently ignores duplicates).

### ⚙️ Execution Flow

```
1️⃣ Run subquery → produces list of client IDs that appear in invoices
2️⃣ Outer query → selects clients whose ID matches any value from that list
```

### ✅ Output

Only clients that have one or more invoices.

---

## ⚙️ 3. Using a `JOIN`

### ✅ Query

```sql
SELECT
    DISTINCT c.*
FROM
    clients c
    INNER JOIN invoices i
        ON c.client_id = i.client_id;
```

### 🔍 Explanation

* The **INNER JOIN** links both tables where their `client_id` matches.
* Because an inner join only returns matching rows, clients with no invoices will automatically be excluded.
* You can add `DISTINCT` to remove duplicates (if clients have multiple invoices).

### ⚙️ Execution Flow

```
clients ── INNER JOIN ── invoices
 ↓
Only clients that appear in both tables
```

---

## ⚙️ 4. Using the `EXISTS` Operator

Now let’s solve the same problem using `EXISTS`.

### ✅ Query

```sql
SELECT
    *
FROM
    clients c
WHERE
    EXISTS (
        SELECT
            client_id
        FROM
            invoices
        WHERE
            client_id = c.client_id
    );
```

### 🔍 Explanation

* For each client in the outer query (`c`), MySQL runs the **subquery** to check if any invoice exists for that client.
* If a matching invoice is found, the `EXISTS` condition returns **TRUE**, and that client is included in the results.

### ⚙️ Execution Flow

```
For each client:
    → Check invoices table for a matching record
    → If found → EXISTS = TRUE → include client
```

---

## ⚙️ 5. Performance Comparison — `IN` vs `EXISTS`

| Operator     | How it Works                              | When to Use                                 | Performance                        |
| ------------ | ----------------------------------------- | ------------------------------------------- | ---------------------------------- |
| **`IN`**     | Returns a list from subquery and compares | When subquery returns a small list          | Can be slower for very large lists |
| **`EXISTS`** | Checks for existence of matching rows     | When subquery might return many rows        | Often faster for large datasets    |
| **`JOIN`**   | Combines tables directly                  | When you also need columns from both tables | Usually fastest overall            |

✅ **Key Point:**

> When the subquery returns a **large result set**, `EXISTS` is generally more efficient than `IN`.

---

## 🧩 6. Exercise — Products That Have *Never Been Ordered*

Now, in the `sql_store` database, let’s find products that have **never** been ordered.

---

### 🧪 Using `NOT IN`

```sql
USE sql_store;

SELECT 
    *
FROM
    products
WHERE
    product_id NOT IN (
        SELECT
            product_id
        FROM
            order_items
    );
```

### 🔍 Explanation

* The subquery returns all product IDs that appear in the `order_items` table (i.e., products that *have been ordered*).
* The outer query then selects all products **not in that list** — those never ordered.

⚠️ **Caution:**
`NOT IN` can produce unexpected results if the subquery contains **NULLs** — so use with care.

---

### 🧪 Using `NOT EXISTS`

```sql
SELECT 
    *
FROM
    products p
WHERE
    NOT EXISTS (
        SELECT
            product_id
        FROM
            order_items
        WHERE
            product_id = p.product_id
    );
```

### 🔍 Explanation

* For each product in `products`, the subquery checks whether a matching record exists in `order_items`.
* If none exists, the subquery returns **FALSE**, and due to `NOT EXISTS`, the product **is included** in the final result.
* This avoids `NULL` pitfalls and is usually faster for large datasets.

---

## 🧠 7. Why `EXISTS` Is Often Better

| Feature                            | `IN`                      | `EXISTS`                          |
| ---------------------------------- | ------------------------- | --------------------------------- |
| Returns data set?                  | Yes (list of values)      | No (boolean TRUE/FALSE)           |
| Handles large result sets well?    | ❌ No                      | ✅ Yes                             |
| Affected by NULL values?           | ❌ Yes                     | ✅ No                              |
| Executes subquery once or per row? | Once                      | Per row                           |
| Real-world analogy                 | "Is this ID in the list?" | "Does at least one record exist?" |

✅ **Performance Tip:**

> Use `EXISTS` when you only need to *check for existence* and not return subquery results.

---

## 💡 8. Summary Table

| Task                   | Best Operator      | Example                  |
| ---------------------- | ------------------ | ------------------------ |
| Clients with invoices  | `EXISTS` or `JOIN` | `WHERE EXISTS (...)`     |
| Products never ordered | `NOT EXISTS`       | `WHERE NOT EXISTS (...)` |
| Small fixed lookup     | `IN`               | `WHERE id IN (1, 2, 3)`  |

---

## 🧩 9. Summary Visualization

```
clients c
│
└── EXISTS (
        SELECT client_id
        FROM invoices
        WHERE client_id = c.client_id
    )
```

✅ → True → Include client in results
❌ → False → Skip client

---

## ✅ Final Takeaways

| Concept             | Description                                                           |
| ------------------- | --------------------------------------------------------------------- |
| **IN**              | Compares a value to a list returned by a subquery                     |
| **EXISTS**          | Checks if *any row exists* matching a condition                       |
| **NOT EXISTS**      | Checks if *no matching row exists*                                    |
| **JOIN**            | Combines tables directly and can often outperform subqueries          |
| **Performance Tip** | Use `EXISTS` for large datasets or when you only need a boolean check |

---

## 10. Queries from the video

```sql
USE sql_invoicing;

SELECT
    *
FROM
	clients
WHERE
	client_id IN (
		SELECT DISTINCT
			client_id
		FROM
			invoices
	);

SELECT
    *
FROM
	clients c
WHERE  EXISTS (
		SELECT
			client_id
		FROM
			invoices
		WHERE
			client_id = c.client_id
	);

```

## 11. Exercise

### Find the products that have never been ordered.

```sql
USE sql_store;

SELECT 
	*
FROM
	products p
WHERE product_id NOT IN (
		SELECT
            product_id
		FROM
			order_items
		WHERE
			product_id = p.product_id
	);

SELECT 
	*
FROM
	products p
WHERE NOT EXISTS (
		SELECT
            product_id
		FROM
			order_items
		WHERE
			product_id = p.product_id
	);
```