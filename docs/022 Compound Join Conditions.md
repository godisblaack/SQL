# 22 Compound Join Conditions

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/SqphSAER6ck?si=Fy-5aAXrxyXoGOwC"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Primary Keys Recap

* A **primary key** uniquely identifies each record in a table.
* Usually, it’s a **single column** (e.g., `customer_id` in the `customers` table).
* Example:

  * `customer_id = 5` → uniquely identifies a customer.

---

## 2. When One Column Is Not Enough

* Sometimes **no single column** can uniquely identify a row.
* Example: **`order_items` table**

  * Columns: `order_id`, `product_id`, `quantity`, `unit_price`.
  * `order_id` → repeated (same order can have multiple products).
  * `product_id` → repeated (same product appears in different orders).
  * **Solution**: Use both together → `(order_id, product_id)`.

👉 This is called a **Composite Primary Key**.

---

## 3. Composite Primary Key

* A **primary key with more than one column**.
* In `order_items`, the composite key is:

  * `(order_id, product_id)`
* Meaning:

  * Each **combination** of `order_id` and `product_id` is unique.
  * Prevents duplicate rows like `(order_id = 2, product_id = 1)` appearing twice.

---

## 4. Why It Matters for Joins

* When joining tables that use composite keys,
  you must **use all key columns in the join condition**.

### Example: `order_items` + `order_item_notes`

* `order_item_notes` stores notes for each product in each order.
* Columns: `note_id`, `order_id`, `product_id`, `note`.
* To join correctly:

  * Match on **both** `order_id` and `product_id`.

---

## 5. Compound Join Condition

```sql
USE sql_store;

SELECT 
    *
FROM
    order_items oi
    JOIN order_item_notes oin 
        ON oi.order_id = oin.order_id
        AND oi.product_id = oin.product_id;
```

### Breakdown:

* `oi.order_id = oin.order_id`
* `AND oi.product_id = oin.product_id`
* ✅ Both conditions ensure we match the correct order item.

---

## 6. Key Concepts

* **Composite Primary Key** = multiple columns together uniquely identify a row.
* **Compound Join Condition** = join tables on all parts of a composite key.
* If you only join on one column:

  * ❌ You may get **duplicate or incorrect matches**.

---

## 7. Visual Example

### `order_items`

| order\_id | product\_id | quantity | unit\_price |
| --------- | ----------- | -------- | ----------- |
| 2         | 1           | 2        | 10.00       |
| 2         | 4           | 1        | 15.00       |
| 2         | 6           | 3        | 20.00       |

### `order_item_notes`

| note\_id | order\_id | product\_id | note           |
| -------- | --------- | ----------- | -------------- |
| 1        | 2         | 1           | Urgent order   |
| 2        | 2         | 1           | Gift packaging |
| 3        | 2         | 4           | Backordered    |

👉 Matching requires **both `order_id` and `product_id`**.

---

✅ With this knowledge, you can safely join tables that use composite keys without errors or mismatches.

---

## Queries from the video

```sql
USE sql_store;

SELECT 
    *
FROM
    order_items oi
        JOIN
    order_item_notes oin ON oi.order_id = oin.order_id
        AND oi.product_id = oin.product_id;
```