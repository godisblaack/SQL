# 19 Joining Across Databases

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/-5Fat0TT1ws?si=L4Ozsa7rfp28YEWH"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## Real-World Context

* In real-world projects, you may need to **work with multiple databases** simultaneously.
* Example scenario:

  * Current database: `sql_store`
  * Another database: `sql_inventory`
* Both databases contain tables, and sometimes you need to **combine data from both**.

---

## Why Work Across Databases?

* Data can be separated for organizational, security, or legacy reasons.
* Example:

  * `sql_store` → contains orders, customers, order\_items.
  * `sql_inventory` → contains products.
* To report on orders with product details, you must join tables **across databases**.

---

## Syntax for Cross-Database Queries

* General format:

  ```
  database_name.table_name
  ```
* If the table is in the **current active database**, no prefix is needed.
* If the table belongs to a **different database**, you must prefix it with the database name.

---

## Example 1: Active Database = `sql_store`

```sql
USE sql_store;

SELECT 
    *
FROM
    order_items oi
    JOIN sql_inventory.products p 
        ON oi.product_id = p.product_id;
```

✅ Explanation:

* `order_items` comes from the current database (`sql_store`) → no prefix needed.
* `products` comes from another database (`sql_inventory`) → requires prefix.

---

## Example 2: Active Database = `sql_inventory`

```sql
USE sql_inventory;

SELECT 
    *
FROM
    sql_store.order_items oi
    JOIN products p 
        ON oi.product_id = p.product_id;
```

✅ Explanation:

* Now, `products` comes from the current database (`sql_inventory`) → no prefix needed.
* `order_items` belongs to `sql_store` → requires prefix.

---

## Key Principle

* **Always prefix tables with their database name if they are not in the active database.**
* Query structure changes depending on which database is currently active.

---

## Best Practices

* Keep related data in the **same database** whenever possible (to avoid complexity).
* Use **database.table.column** fully qualified names in complex systems for clarity.
* Be consistent with **aliases** to keep code readable:

  * Example: `oi` for `order_items`, `p` for `products`.

---

## Key Takeaways

* You can seamlessly join tables from multiple databases in MySQL.
* Prefix with `database_name.` only when necessary.
* The **active database (`USE database;`)** determines which tables need prefixing.

---

## Queries from the video

```sql
USE sql_store;

SELECT 
    *
FROM
    order_items oi
        JOIN
    sql_inventory.products p ON oi.product_id = p.product_id;

-- The query will be different based on the current active database as shown below.
USE sql_inventory;

SELECT 
    *
FROM
    sql_store.order_items oi
        JOIN
    products p ON oi.product_id = p.product_id;
```