# 39 Deleting Rows

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/MEfQTJhJHxs?si=c31pw7YX37wS75v9"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🧠 1. Introduction

After learning how to **insert** and **update** data, the final essential data manipulation operation is **deleting records**.

In SQL, you can remove one or multiple rows from a table using the **`DELETE FROM`** statement.

---

## ⚙️ 2. Syntax of the `DELETE` Statement

```sql
DELETE FROM table_name
WHERE condition;
```

| Clause                   | Description                                  |
| ------------------------ | -------------------------------------------- |
| `DELETE FROM table_name` | Specifies which table to delete records from |
| `WHERE condition`        | (Optional) Specifies which rows to delete    |

⚠️ **Warning:**
If you **omit the `WHERE` clause**, **all rows in the table** will be deleted.

---

## 🧩 3. Example – Deleting a Specific Record

Suppose we want to delete an invoice with an `invoice_id` of **1** from the `invoices` table.

```sql
USE sql_invoicing;

DELETE FROM
	invoices
WHERE
	invoice_id = 1;
```

✅ This removes only the record with `invoice_id = 1`.
✅ Other records remain untouched.

---

## 🚨 4. Deleting Without a `WHERE` Clause (⚠️ Dangerous)

```sql
DELETE FROM invoices;
```

⚠️ **This deletes *every record* in the `invoices` table!**
Use it only when you truly intend to clear the entire table (e.g., before archiving or resetting test data).

💡 If you only want to remove all rows but keep the table structure, you can also use:

```sql
TRUNCATE TABLE invoices;
```

This is faster and resets any **auto-increment** values, but it cannot be rolled back in some databases.

---

## 🧠 5. Using Subqueries in `DELETE` Statements

Just like in `UPDATE`, you can use a **subquery** to dynamically determine *which records to delete*.

For example, suppose you want to delete all invoices for a client named **“Myworks.”**

### Step 1: Find the client

```sql
SELECT *
FROM clients
WHERE name = 'Myworks';
```

→ Suppose it returns `client_id = 2`.

---

### Step 2: Use a subquery in the DELETE statement

Instead of hardcoding the ID, use the subquery directly:

```sql
DELETE FROM
	invoices
WHERE
	client_id = (
		SELECT
			client_id
		FROM
			clients
		WHERE
			name = 'Myworks'
	);
```

✅ MySQL first executes the **subquery** to get the `client_id` for “Myworks.”
✅ Then it deletes all invoices with that matching `client_id`.

---

## 🔁 6. When Subquery Returns Multiple Results

If your subquery returns **multiple client IDs**, you should use the `IN` operator instead of `=`:

```sql
DELETE FROM
	invoices
WHERE
	client_id IN (
		SELECT
			client_id
		FROM
			clients
		WHERE
			state IN ('CA', 'NY')
	);
```

✅ Deletes invoices for all clients located in **California** or **New York**.

---

## 🧰 7. Best Practices for Safe Deletes

| Best Practice                      | Description                                                                       |
| ---------------------------------- | --------------------------------------------------------------------------------- |
| **Preview first**                  | Always run a `SELECT` query before deleting to see which records will be affected |
| **Use transactions**               | Wrap delete statements inside a transaction (`BEGIN` / `COMMIT`) for safety       |
| **Use `LIMIT` (if supported)**     | To delete in smaller batches when dealing with large tables                       |
| **Backup important data**          | Before running deletes in production environments                                 |
| **Avoid deleting without `WHERE`** | Unless you intentionally want to clear the entire table                           |

---

### ✅ Example: Preview Before Deleting

Before executing a delete:

```sql
SELECT *
FROM invoices
WHERE client_id = (
	SELECT client_id
	FROM clients
	WHERE name = 'Myworks'
);
```

Once you verify the correct records, then execute your delete statement.

---

## 🧩 8. Summary Table

| Operation                         | Query                                                                                                 | Description                             |
| --------------------------------- | ----------------------------------------------------------------------------------------------------- | --------------------------------------- |
| Delete a specific record          | `DELETE FROM invoices WHERE invoice_id = 1;`                                                          | Deletes one record                      |
| Delete by subquery                | `DELETE FROM invoices WHERE client_id = (SELECT client_id FROM clients WHERE name='Myworks');`        | Deletes all invoices for the client     |
| Delete multiple clients’ invoices | `DELETE FROM invoices WHERE client_id IN (SELECT client_id FROM clients WHERE state IN ('CA','NY'));` | Deletes invoices for multiple states    |
| Delete all data                   | `DELETE FROM invoices;`                                                                               | Deletes everything (use with caution)   |
| Truncate all data                 | `TRUNCATE TABLE invoices;`                                                                            | Deletes all data, resets auto-increment |

---

## 🧠 9. Key Takeaways

* `DELETE FROM` is used to remove rows from a table.
* The `WHERE` clause filters which records get deleted.
* Subqueries can make deletions **dynamic and flexible**.
* Always **preview** before deleting and **avoid full deletes** unless necessary.
* Use `IN` when your subquery returns **multiple results**.

---

### ✅ 10. Example Recap (from the tutorial)

```sql
USE sql_invoicing;

-- Delete specific invoice
DELETE FROM
	invoices
WHERE
	invoice_id = 1;

-- Find client ID by name
SELECT
	*
FROM
	clients
WHERE
	name = 'Myworks';

-- Delete all invoices for that client
DELETE FROM
	invoices
WHERE
	client_id = (
		SELECT
			client_id
		FROM
			clients
		WHERE
			name = 'Myworks'
	);
```

---

## 11. Queries from the video

```sql
USE sql_invoicing;

DELETE FROM
	invoices
WHERE
	invoice_id = 1;
    
SELECT
	*
FROM
	clients
WHERE
	name = 'Myworks';

DELETE FROM
	invoices
WHERE
	client_id = (
						
					SELECT
						*
					FROM
						clients
					WHERE
						name = 'Myworks'
				);
```