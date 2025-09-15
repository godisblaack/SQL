# 21 Joining Multiple Tables

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/2FI2fWnMznQ?si=F-njbYuDcaWLWquO"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## Real-World Need

* Tables are often **normalized** into separate entities.
* Example: In `sql_store`, the `orders` table stores:

  * `customer_id` → references the `customers` table.
  * `status` → references the `order_statuses` table.
* To create meaningful **reports**, we need to join all three:

  * `orders`
  * `customers`
  * `order_statuses`

---

## Step 1 – Basic Join with Two Tables

```sql
USE sql_store;

SELECT 
    *
FROM
    orders o
    JOIN customers c 
        ON o.customer_id = c.customer_id;
```

✅ Joins orders with customers.
❌ Still missing the **status name**.

---

## Step 2 – Join a Third Table

```sql
SELECT 
    *
FROM
    orders o
    JOIN customers c 
        ON o.customer_id = c.customer_id
    JOIN order_statuses os 
        ON o.status = os.order_status_id;
```

✅ Now we have data from:

* `orders` (order details)
* `customers` (customer info)
* `order_statuses` (status names)

⚠️ Output is too cluttered → we should handpick useful columns.

---

## Step 3 – Select Useful Columns

```sql
SELECT 
    o.order_id,
    o.order_date,
    c.first_name,
    c.last_name,
    os.name AS status
FROM
    orders o
    JOIN customers c 
        ON o.customer_id = c.customer_id
    JOIN order_statuses os 
        ON o.status = os.order_status_id;
```

✅ Final readable report:

* `order_id`
* `order_date`
* `customer name`
* `status` (aliased for clarity)

---

## Key Principles

* You can **chain multiple JOINs**.
* Each `JOIN` needs a **condition** (`ON`).
* Use **aliases** (`o`, `c`, `os`) to keep queries short.
* You can join **3, 5, or even 10+ tables** depending on the schema.

---

## Exercise – Payments with Clients and Payment Methods

### Step 1 – Switch Database

```sql
USE sql_invoicing;
```

---

### Step 2 – Join All Tables

```sql
SELECT 
    *
FROM
    payments p
    JOIN clients c 
        ON p.client_id = c.client_id
    JOIN payment_methods pm 
        ON p.payment_method = pm.payment_method_id;
```

✅ Joins:

* `payments → clients` (to get client details).
* `payments → payment_methods` (to get payment method names).

---

### Step 3 – Select Specific Columns

```sql
SELECT 
    p.date, 
    p.invoice_id, 
    p.amount, 
    c.name AS client_name, 
    pm.name AS method
FROM
    payments p
    JOIN clients c 
        ON p.client_id = c.client_id
    JOIN payment_methods pm 
        ON p.payment_method = pm.payment_method_id;
```

✅ Final Output Example:

| date       | invoice\_id | amount | client\_name | method        |
| ---------- | ----------- | ------ | ------------ | ------------- |
| 2023-01-05 | 101         | 500    | Acme Corp    | Credit Card   |
| 2023-01-07 | 102         | 1200   | XYZ Ltd      | Wire Transfer |

---

## Takeaways

* **JOINs can be chained** to combine multiple tables.
* Always use **aliases** for readability.
* Pick only the needed columns → avoids clutter.
* Joins across 3+ tables are **very common in production SQL**.

---

## Queries from the video

```sql
USE sql_store;

SELECT 
    *
FROM
    orders o
        JOIN
    customers c ON o.customer_id = c.customer_id
        JOIN
    order_statuses os ON o.status = os.order_status_id;
    
SELECT 
    o.order_id,
    o.order_date,
    c.first_name,
    c.last_name,
    os.name AS status
FROM
    orders o
        JOIN
    customers c ON o.customer_id = c.customer_id
        JOIN
    order_statuses os ON o.status = os.order_status_id;
```

## Exercise

### Payments + Clients + Payment Methods

### Switching to Another Database
```sql
USE sql_invoicing;
```
- Sets the active database to `sql_invoicing`.

---

### All Columns
```sql
SELECT 
    *
FROM
    payments AS p
    JOIN clients AS c 
        ON p.client_id = c.client_id
    JOIN payment_methods pm 
        ON p.payment_method = pm.payment_method_id;
```
- **`p`** = alias for `payments`
- **`c`** = alias for `clients`
- **`pm`** = alias for `payment_methods`
- Joins:
  - `payments.client_id` → `clients.client_id`
  - `payments.payment_method` → `payment_methods.payment_method_id`

---

### Selected Columns
```sql
SELECT 
    p.date, 
    p.invoice_id, 
    p.amount, 
    c.name, 
    pm.name
FROM
    payments AS p
    JOIN clients AS c 
        ON p.client_id = c.client_id
    JOIN payment_methods pm 
        ON p.payment_method = pm.payment_method_id;
```
- Shows:
  - Payment date
  - Invoice ID
  - Payment amount
  - Client name
  - Payment method name