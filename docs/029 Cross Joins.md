# 29 Cross Joins

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/_sTJo_uwc24?si=IFkkG4CKT0SsE5LO"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. What is a Cross Join?

* A **Cross Join** combines **every record** in the first table with **every record** in the second table.
* This produces the **Cartesian product** of the two tables.
* **No join condition** is used.

👉 Example:

* 10 customers × 5 products = **50 rows** in the result.

---

## 2. Explicit Cross Join Syntax

```sql
USE sql_store;

SELECT 
    c.first_name AS customer,
    p.name AS product
FROM customers c
CROSS JOIN products p
ORDER BY c.first_name;
```

✅ Each customer is paired with every product.
✅ Explicit `CROSS JOIN` makes the intent **clear**.

---

## 3. Implicit Cross Join Syntax

```sql
SELECT 
    c.first_name AS customer,
    p.name AS product
FROM customers c, products p
ORDER BY c.first_name;
```

* This produces the **same result** as the explicit version.
* But it can be confusing because it looks like a regular join without a `WHERE` clause.
* ⚠️ Easy to make mistakes (forgetting the condition can turn a normal join into a cross join).

---

## 4. When to Use Cross Joins

* Cross joins aren’t commonly used in business queries, but they’re **very useful** in generating **combinations**.
* Example:

  * Table `sizes`: `S`, `M`, `L`
  * Table `colors`: `Red`, `Blue`, `Green`
  * Cross join → all possible `(size, color)` combinations.

---

## 5. Exercise – Cross Join Between Shippers and Products

### Using Implicit Syntax:

```sql
SELECT 
    sh.name AS shipper,
    p.name AS product
FROM shippers sh, products p
ORDER BY sh.name;
```

### Using Explicit Syntax:

```sql
SELECT 
    sh.name AS shipper,
    p.name AS product
FROM shippers sh
CROSS JOIN products p
ORDER BY sh.name;
```

✅ Both queries return all combinations of **shippers × products**.

---

## 6. Key Takeaways

* **Cross Join = Cartesian Product**.
* Use **explicit syntax** (`CROSS JOIN`) for clarity.
* Implicit syntax works but can be misleading.
* Practical use case: **generate all possible combinations** (colors × sizes, shippers × products, etc.).

---

## Queries from the video

```sql
USE sql_store;

SELECT 
    c.first_name AS customer, p.name AS product
FROM
    customers c
        CROSS JOIN
    products p
ORDER BY c.first_name;

SELECT 
    c.first_name AS customer, o.order_id
FROM
    customers c,
    products p
ORDER BY c.first_name;

SELECT 
    c.first_name AS customer, p.name AS product
FROM
    customers c,
    products p
ORDER BY c.first_name;
```

## Exercise

```sql
use sql_store;

SELECT 
    *
FROM
    shippers,
    products;
    
SELECT 
    *
FROM
    shippers
        CROSS JOIN
    products;

-- Solution by Mosh

SELECT 
    sh.name AS shipper, p.name AS product
FROM
    shippers sh,
    products p
ORDER BY sh.name;

SELECT 
    sh.name AS shipper, p.name AS product
FROM
    shippers sh
        CROSS JOIN
    products p
ORDER BY sh.name;
```