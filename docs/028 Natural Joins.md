# 28 Natural Joins

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/9sya7TjX118?si=4Sj1a_gQtIJ5U-SY"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. What is a Natural Join?

* A **`NATURAL JOIN`** automatically joins two tables based on all columns that have the **same name** in both tables.
* You don’t have to explicitly write the join condition (`ON` or `USING`).

---

## 2. Example

```sql
USE sql_store;

SELECT 
    o.order_id, 
    c.first_name
FROM orders o
NATURAL JOIN customers c;
```

### ✅ What happens here:

* MySQL looks at `orders` and `customers`.
* Finds that both have the column `customer_id`.
* Joins the two tables on that column automatically.

Result → Each order with the first name of the customer who placed it.

---

## 3. Why It’s Risky

* With `NATURAL JOIN`, you **don’t control** the join condition.
* If two tables share multiple column names (e.g., `customer_id`, `status`, `created_at`), MySQL will join on **all of them**, which can lead to **unexpected or wrong results**.
* This makes queries harder to **debug** and maintain.

---

## 4. Best Practice

* ⚠️ Avoid using `NATURAL JOIN` in production code.
* ✅ Use `JOIN ... ON` or `JOIN ... USING` instead, so your join logic is **explicit and predictable**.

---

## 5. Key Takeaways

* **Natural Join** = shortcut (automatic join on same-name columns).
* ❌ Dangerous when tables share multiple common columns.
* ✅ Always better to be explicit (`ON` / `USING`).
* Use `NATURAL JOIN` only if you’re reading legacy SQL or quick testing.

---

# Queries from the video

```sql
USE sql_store;

SELECT 
    o.order_id, c.first_name
FROM
    orders o
        NATURAL JOIN
    customers c;
```