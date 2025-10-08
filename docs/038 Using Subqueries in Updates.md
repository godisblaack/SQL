# 38 Using Subqueries in Updates

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/h-X_NIN-kno?si=cSRc7owdA55dsNSf"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🧠 1. What Are Subqueries?

A **subquery** (also called an *inner query*) is a `SELECT` statement nested **inside another SQL statement** such as `SELECT`, `INSERT`, `UPDATE`, or `DELETE`.

They are powerful because they let you:

* Dynamically fetch data from other tables.
* Use that data as part of your main query.
* Avoid hardcoding values (like specific IDs).

---

## ⚙️ 2. Syntax Overview

A subquery inside an `UPDATE` looks like this:

```sql
UPDATE table_name
SET column_name = value
WHERE column_name = (
    SELECT column_name
    FROM other_table
    WHERE condition
);
```

✅ The subquery runs **first**,
✅ The result is then used in the main `UPDATE` statement.

---

## 📊 3. Example – Updating Invoices Based on Client Name

In previous examples, we updated all invoices for **client ID = 3**.

But what if you **don’t know the client ID** — only their name?

### Step 1: Find the client ID

```sql
SELECT client_id
FROM clients
WHERE name = 'Myworks';
```

→ Result: `client_id = 2`

### Step 2: Use it in a subquery

Now we can use that `SELECT` inside an `UPDATE`:

```sql
USE sql_invoicing;

UPDATE 
	invoices
SET 
	payment_total = invoice_total * 0.5,
    payment_date = due_date
WHERE
	client_id = (
		SELECT client_id
		FROM clients
		WHERE name = 'Myworks'
	);
```

✅ The subquery returns a **single value** (client_id 2)
✅ The outer query updates all invoices for that client

---

## 🧩 4. When Subquery Returns Multiple Results

Sometimes, your subquery may return **more than one record**.

For example, suppose you want to update invoices for **all clients in California or New York**.

### Step 1: Run the subquery alone first

```sql
SELECT client_id
FROM clients
WHERE state IN ('CA', 'NY');
```

→ Returns multiple client IDs: `1, 3`

### Step 2: Use `IN` instead of `=`

Since multiple values are returned, replace `=` with `IN`:

```sql
UPDATE 
	invoices
SET 
	payment_total = invoice_total * 0.5,
    payment_date = due_date
WHERE
	client_id IN (
		SELECT client_id
		FROM clients
		WHERE state IN ('CA', 'NY')
	);
```

✅ Updates all invoices for **clients located in CA or NY**
✅ The subquery provides the list of matching client IDs

---

## 🧠 5. Best Practice – Test Your Query Before Updating

Before executing an `UPDATE`, always **preview the affected records** using a `SELECT` statement.

Example:

```sql
SELECT *
FROM invoices
WHERE payment_date IS NULL;
```

This shows which rows will be updated if you later run:

```sql
UPDATE invoices
SET payment_total = invoice_total * 0.5,
    payment_date = due_date
WHERE payment_date IS NULL;
```

✅ Prevents accidental mass updates
✅ Lets you confirm conditions are correct before writing changes

---

## ⚙️ 6. General Rules for Subqueries in Updates

| Rule                                        | Description                                   |
| ------------------------------------------- | --------------------------------------------- |
| Must be enclosed in parentheses `( )`       | Required syntax for subqueries                |
| Can return one or multiple rows             | Use `=` for one result, `IN` for multiple     |
| Executes before the outer query             | The subquery result is used in the main query |
| Can include joins, filters, or aggregations | Very flexible logic                           |
| Useful for data-driven updates              | Avoids hardcoding values                      |

---

## 🧩 7. Exercise – Gold Customers in the `sql_store` Database

### 🧾 Task

Update the `comments` column in the `orders` table for all **customers who have more than 3,000 points**.
If such a customer has placed an order, mark them as **“Gold customer.”**

---

### ✅ Step 1: Identify Gold Customers

```sql
USE sql_store;

SELECT *
FROM customers
WHERE points > 3000;
```

→ Suppose this returns customer IDs 2, 4, and 6.

---

### ✅ Step 2: Get Only the IDs

```sql
SELECT customer_id
FROM customers
WHERE points > 3000;
```

---

### ✅ Step 3: Use It as a Subquery in an Update

```sql
UPDATE
	orders
SET
	comments = 'Gold customer'
WHERE
	customer_id IN (
		SELECT
			customer_id
		FROM
			customers
		WHERE
			points > 3000
	);
```

✅ Updates all orders made by “Gold customers”
✅ Uses a **subquery** to dynamically find those customers
✅ Automatically applies to new high-point customers too (no need to manually change IDs)

---

## 🧾 8. Verification Step

Before running the update:

```sql
SELECT *
FROM orders
WHERE customer_id IN (
	SELECT customer_id
	FROM customers
	WHERE points > 3000
);
```

After confirming, then execute the update query above.

---

## 🧠 9. Summary Table

| Concept                          | Example                                                                         | Notes                |
| -------------------------------- | ------------------------------------------------------------------------------- | -------------------- |
| Subquery returns one value       | `WHERE client_id = (SELECT client_id FROM clients WHERE name='Myworks')`        | Use `=`              |
| Subquery returns multiple values | `WHERE client_id IN (SELECT client_id FROM clients WHERE state IN ('CA','NY'))` | Use `IN`             |
| Test before update               | `SELECT * FROM invoices WHERE payment_date IS NULL;`                            | Avoids mistakes      |
| Real-world use case              | Marking customers as “Gold,” updating invoices by location                      | Great for automation |

---

## ✅ 10. Quick Recap

* 🔹 Subqueries can be used inside `UPDATE` statements for **dynamic updates**.
* 🔹 Use **`=`** when your subquery returns **one value**.
* 🔹 Use **`IN`** when your subquery returns **multiple values**.
* 🔹 Always test your subquery with a **`SELECT`** before running updates.
* 🔹 This technique keeps your SQL flexible, reusable, and safe.

---

## 11. Queries from the video

```sql
USE sql_invoicing;

UPDATE 
	invoices
SET 
	payment_total = invoice_total * 0.5,
    payment_date = due_date
WHERE
	client_id = (
				SELECT
					client_id
				FROM
					clients
				 WHERE
					name = 'Myworks'
	);
    
UPDATE 
	invoices
SET 
	payment_total = invoice_total * 0.5,
    payment_date = due_date
WHERE
	client_id IN (
				SELECT
					client_id
				FROM
					clients
				 WHERE
					state IN ('CA', 'NY')
	);

-- Query for confirmation before running the update statement

SELECT
	*
FROM
	invoices
WHERE
	payment_date IS NULL;

-- Update query

UPDATE 
	invoices
SET 
	payment_total = invoice_total * 0.5,
    payment_date = due_date
    
WHERE
	payment_date IS NULL;

-- Query for confirmation before running the update statement
SELECT
	*
FROM
	invoices
WHERE
	payment_date IS NULL;
```

---

## 12. Exercise

### In orders table of sql_store database, update comments for all the customers who has more than 3000 points. If the customer has placed an order regard them as Gold customer.

```sql

USE sql_store;

UPDATE
	orders
SET
	comments = 'Gold customer'
WHERE
	customer_id IN (
		SELECT
			customer_id
		FROM
			customers
		WHERE
			points > 3000
	);
```