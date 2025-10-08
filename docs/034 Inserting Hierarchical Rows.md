# 34 Inserting Hierarchical Rows

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/RQCrkdkv8so?si=N0AKZh_BLWWR_OBw"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Parent–Child Relationships

* In relational databases, one table often represents a **parent entity**, and another represents its **children**.
* **Example**:

  * `orders` = parent table (general order info: ID, customer, date, status, etc.).
  * `order_items` = child table (specific products within an order: product ID, quantity, price).
* **One order** → can have **multiple order items**.

---

## 2. Step 1 – Insert into Parent Table (`orders`)

Only required columns need to be provided.

* `order_id` → Auto Increment (skip it).
* Required: `customer_id`, `order_date`, `status`.

```sql
USE sql_store;

INSERT INTO orders (customer_id, order_date, status)
VALUES (1, '2019-01-02', 1);
```

✅ This creates a new order.
❗ But we don’t yet know the auto-generated `order_id`.

---

## 3. Step 2 – Get the Last Inserted ID

MySQL provides the built-in function:

```sql
SELECT LAST_INSERT_ID();
```

* Returns the **primary key** of the most recent `AUTO_INCREMENT` record created in the current session.
* Useful for linking parent and child records.

---

## 4. Step 3 – Insert into Child Table (`order_items`)

Child rows must reference the parent order.
We can use `LAST_INSERT_ID()` directly inside the `VALUES` clause.

```sql
INSERT INTO order_items
VALUES 
    (LAST_INSERT_ID(), 1, 1, 2.95),  -- Order 13, Product 1, qty 1, price 2.95
    (LAST_INSERT_ID(), 2, 1, 3.95); -- Order 13, Product 2, qty 1, price 3.95
```

✅ Both items are correctly tied to the new order.

---

## 5. End Result

* `orders` table → new row (e.g., `order_id = 13`).
* `order_items` table → two rows referencing `order_id = 13`.

This ensures **referential integrity**: every item belongs to a valid order.

---

## 6. Key Takeaways

* Use `LAST_INSERT_ID()` to link inserts between parent and child tables.
* Always insert into the **parent first**, then the **children**.
* This approach works for **1-to-many** relationships (orders → order_items, invoices → invoice_lines, etc.).

---

## 7. Queries from the video

```sql
USE sql_store;

INSERT INTO 
    orders (
        customer_id, 
        order_date, 
        status
    )
VALUES (1, '2019-01-02', 1);

-- To get the last inserted id use:
SELECT LAST_INSERT_ID();

INSERT INTO 
    order_items
VALUES 
	(LAST_INSERT_ID(), 1, 1, 2.95),
	(LAST_INSERT_ID(), 2, 1, 3.95);
```