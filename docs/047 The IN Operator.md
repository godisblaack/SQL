# 47 The IN Operator

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/nIe59Ycfh0o?si=lH29CfWOpkTmrv36"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🧩 1. What is a Subquery with the `IN` Operator?

A **subquery with the `IN` (or `NOT IN`) operator** allows you to compare a value from the outer query against a **list of values** returned by the inner query.

This is useful when you need to check whether something **exists** or **does not exist** in another dataset — for example, products that have **never been ordered** or clients that **have no invoices**.

---

## ⚙️ 2. Basic Syntax

```sql
SELECT 
    column_list
FROM 
    table_name
WHERE 
    column_name [NOT] IN (
        SELECT column_name
        FROM another_table
        WHERE condition
    );
```

### 💡 Notes:

* The **subquery** inside `IN` returns a **list of values**.
* The **outer query** compares each record against this list.
* Use **`IN`** to match values and **`NOT IN`** to exclude them.
* Always ensure the **data types** between both columns match.

---

## 🧩 3. Example — Products Never Ordered

Let’s find all products in the `sql_store` database that **have never been ordered**.

```sql
USE sql_store;

SELECT 
    *
FROM
    products
WHERE
    product_id NOT IN (
        SELECT DISTINCT 
            product_id
        FROM
            order_items
    );
```

### 🔍 Explanation:

* The **inner query** selects the **unique list of product IDs** that appear in the `order_items` table (i.e., products that have been ordered).
* The **outer query** retrieves all products from the `products` table **whose IDs are not in that list**.
* This gives us products that **were never ordered**.

✅ **Output:**

| product_id | name    | unit_price | ... |
| ---------- | ------- | ---------- | --- |
| 7          | Avocado | 3.50       | ... |

---

## 🧠 4. Understanding How the `IN` Subquery Works

1. The **subquery** runs first and returns a **list of values** (a column of IDs).
2. The **outer query** checks each record in its table to see if its value exists (or doesn’t exist) in that list.
3. When combined with `NOT IN`, it filters out all rows that appear in the subquery’s results.

💡 Think of it as:

> “Show me all products that **are not among** the products that were ordered.”

---

## 🧱 5. Comparison with Previous Example

| Previous Lesson                               | This Lesson                           |
| --------------------------------------------- | ------------------------------------- |
| Subquery returned a **single value**          | Subquery returns a **list of values** |
| Used a **comparison operator** like `>`       | Uses the **`IN` / `NOT IN`** operator |
| Example: Find employees earning above average | Example: Find products never ordered  |

So, this example introduces **multi-value subqueries**, which return multiple results instead of a single scalar.

---

## 🧩 6. Exercise — Clients Without Invoices

Let’s write a query in the `sql_invoicing` database to find clients who **don’t have any invoices**.

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

* The **inner query** retrieves all client IDs that **appear in the `invoices` table**.
* The **outer query** returns all clients whose IDs are **not found** in that list.
* This gives us the clients who **never made an invoice**.

✅ **Output:**

| client_id | name         | city        |
| --------- | ------------ | ----------- |
| 10        | Modern Foods | Los Angeles |

---

## ⚙️ 7. Key Rules and Notes

1. Use `IN` when checking for **membership** in a list.
2. Use `NOT IN` to **exclude** matching results.
3. `DISTINCT` is often helpful to avoid duplicate values in the subquery.
4. Ensure the subquery returns only **one column** (the column you are comparing).
5. Avoid using `NOT IN` with columns that may contain **NULLs**, as it can lead to unexpected results.

---

## 💡 8. Key Takeaways

| Concept              | Description                                       |
| -------------------- | ------------------------------------------------- |
| **`IN` Subquery**    | Compares values against a list from another query |
| **`NOT IN`**         | Finds values not present in another list          |
| **Returns**          | A set (list) of results instead of a single value |
| **Typical Use Case** | Finding missing or unlinked records               |
| **Performance Tip**  | Use `EXISTS` for better performance on large data |

---

## 🚀 9. Real-World Use Cases

* Products never ordered
* Customers who haven’t placed an order
* Employees not assigned to any project
* Clients with no invoices
* Inventory items that were never sold

---

## 10. Queries from the video

```sql
USE sql_store;

SELECT 
    *
FROM
    products
WHERE
    product_id NOT IN (
		SELECT DISTINCT 
			product_id
        FROM
            order_items
	);
```

## 11. Exercise

### Find clients without invoices

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