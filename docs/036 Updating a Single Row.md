# 36 Updating a Single Row

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/Yzjt9E4sIC8?si=a_SlqXCYBdjze7xO"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🧠 1. Introduction

The `UPDATE` statement in SQL is used to **modify existing data** in a table.
Unlike `INSERT`, which adds new rows, `UPDATE` allows you to change values in existing records — either for a **single record** or **multiple records** at once.

---

## ⚙️ 2. Syntax of the `UPDATE` Statement

```sql
UPDATE table_name
SET column1 = value1,
    column2 = value2,
    ...
WHERE condition;
```

### 📘 Explanation:

| Clause     | Description                                                            |
| ---------- | ---------------------------------------------------------------------- |
| **UPDATE** | Specifies which table you want to modify                               |
| **SET**    | Defines one or more columns and their new values                       |
| **WHERE**  | Filters which rows to update (if omitted, *all rows* will be updated!) |

> ⚠️ **Always include a `WHERE` clause** when updating — unless you really want to change *every* row in the table.

---

## 🧩 3. Example 1 – Updating a Record

Let’s say we have an **`invoices`** table, and the first invoice (`invoice_id = 1`) was recorded incorrectly:

* It shows no payment (`payment_total = 0`, `payment_date = NULL`)
* But the client actually paid **$10** on **March 1, 2019**

We can fix that:

```sql
USE sql_invoicing;

UPDATE 
	invoices
SET 
	payment_total = 10,
    payment_date = '2019-03-01'
WHERE
	invoice_id = 1;
```

✅ Updates `payment_total` → `10`
✅ Updates `payment_date` → `2019-03-01`
✅ Only affects the record with `invoice_id = 1`

---

## 🔁 4. Example 2 – Restoring Defaults and Nulls

If we updated the wrong record, we can revert it using either:

* **Literal values**
* **`DEFAULT`** keyword (to reset a column to its default value)
* **`NULL`** keyword (to clear a nullable column)

```sql
UPDATE 
	invoices
SET 
	payment_total = 0,        -- literal value
    payment_date = NULL       -- clears the column
WHERE
	invoice_id = 1;
```

### 🟡 Alternate Form (Using DEFAULT):

```sql
UPDATE 
	invoices
SET 
	payment_total = DEFAULT,  -- uses the default defined in schema (0)
    payment_date = NULL
WHERE
	invoice_id = 1;
```

✅ `payment_total` resets to **0** (default)
✅ `payment_date` resets to **NULL**

---

## 💡 Understanding Default and Null Values

| Term         | Meaning                                                               |
| ------------ | --------------------------------------------------------------------- |
| **DEFAULT**  | Uses the value specified in the column definition (e.g., `DEFAULT 0`) |
| **NULL**     | Means *no value* or *unknown*                                         |
| **NOT NULL** | Restricts column from accepting NULLs                                 |

You can check column defaults by opening the table in **Design Mode** or querying the schema.

---

## 📊 5. Example 3 – Using Expressions in Updates

You’re not limited to literal values — you can use **expressions**, **calculations**, or even **values from other columns**.

### Scenario:

Client for invoice #3 paid **50%** of the total amount on the **due date**.

```sql
UPDATE 
	invoices
SET 
	payment_total = invoice_total * 0.5,
    payment_date = due_date
WHERE
	invoice_id = 3;
```

### 🧩 How it Works:

| Column          | New Value             | Explanation                             |
| --------------- | --------------------- | --------------------------------------- |
| `payment_total` | `invoice_total * 0.5` | Calculates 50% of the invoice total     |
| `payment_date`  | `due_date`            | Copies value from the `due_date` column |

✅ Dynamically calculates the new payment
✅ Uses column values from the same row
✅ Updates only the record with `invoice_id = 3`

> ⚠️ Note: MySQL may truncate decimal digits depending on the column’s **data type** (e.g., `INT` vs `DECIMAL`). You’ll learn more about this in the data types section.

---

## ⚙️ 6. Updating Multiple Columns

You can update as many columns as needed — separate each column-value pair with a comma.

```sql
UPDATE table_name
SET col1 = val1,
    col2 = val2,
    col3 = val3
WHERE condition;
```

Example:

```sql
UPDATE employees
SET salary = salary * 1.05,
    status = 'Active',
    updated_at = NOW()
WHERE department = 'Sales';
```

---

## 🧠 7. Key Points & Best Practices

| Concept                 | Description                                          |
| ----------------------- | ---------------------------------------------------- |
| **Use WHERE carefully** | Without it, *every record* will be modified          |
| **Use expressions**     | You can compute new values dynamically               |
| **Use DEFAULT/NULL**    | Great for resetting data cleanly                     |
| **Verify first**        | Always preview rows using a `SELECT` before `UPDATE` |
| **Backups**             | Take backups of critical tables before large updates |

---

## 🧾 8. Full Example Summary

### Step 1 — Update invoice #1 with new payment info:

```sql
UPDATE invoices
SET payment_total = 10,
    payment_date = '2019-03-01'
WHERE invoice_id = 1;
```

### Step 2 — Revert the incorrect update:

```sql
UPDATE invoices
SET payment_total = DEFAULT,
    payment_date = NULL
WHERE invoice_id = 1;
```

### Step 3 — Record a 50% payment for invoice #3:

```sql
UPDATE invoices
SET payment_total = invoice_total * 0.5,
    payment_date = due_date
WHERE invoice_id = 3;
```

---

## 🔍 9. Visual Recap

| Invoice ID   | Before                                              | After                                                             | Notes             |
| ------------ | --------------------------------------------------- | ----------------------------------------------------------------- | ----------------- |
| 1            | `payment_total = 0`, `payment_date = NULL`          | `payment_total = 10`, `payment_date = '2019-03-01'`               | Updated payment   |
| 1 (reverted) | `payment_total = 10`, `payment_date = '2019-03-01'` | `payment_total = 0`, `payment_date = NULL`                        | Reverted          |
| 3            | `payment_total = 0`, `payment_date = NULL`          | `payment_total = 50% of invoice_total`, `payment_date = due_date` | Calculated update |

---

## 🧩 10. Summary Table

| Keyword / Function         | Purpose                            |
| -------------------------- | ---------------------------------- |
| `UPDATE`                   | Modify existing data in a table    |
| `SET`                      | Specify new column values          |
| `WHERE`                    | Define which rows to update        |
| `DEFAULT`                  | Reset column to default value      |
| `NULL`                     | Clear column (set to null)         |
| Expressions (`col1 * 0.5`) | Perform calculations during update |

---

## ✅ 11. Quick Takeaways

* 🔹 Use `UPDATE ... SET ... WHERE ...` to modify rows.
* 🔹 Use expressions for dynamic calculations.
* 🔹 Always test your condition with a `SELECT` first.
* 🔹 Use `DEFAULT` and `NULL` to reset values.
* 🔹 Avoid missing `WHERE` — it can update *all records!*

---

## 12. Queries from the video

```sql
USE sql_invoicing;

UPDATE 
	invoices
SET 
	payment_total = 10,
    payment_date = '2019-03-01'
WHERE
	invoice_id = 1;
    
UPDATE 
	invoices
SET 
	payment_total = 0, -- We can also use the DEFAULT keyword
    payment_date = NULL
WHERE
	invoice_id = 1;
    
UPDATE 
	invoices
SET 
	payment_total = invoice_total * 0.5,
    payment_date = due_date
WHERE
	invoice_id = 3;
```