# 23 Implicit Join Syntax

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/vtjG-I2dNPo?si=Whh1S_SbnCM_hxYG"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Explicit Join (Recommended)

The **explicit join syntax** is the standard and safest way to join tables.

```sql
USE sql_store;

SELECT 
    *
FROM
    orders o
    JOIN customers c 
        ON o.customer_id = c.customer_id;
```

* Uses `JOIN ... ON ...` syntax.
* Forces you to write the **join condition**.
* ✅ If you forget the condition → you get a **syntax error** (safe).

---

## 2. Implicit Join Syntax

MySQL also supports an **older style** of writing joins:

```sql
SELECT 
    *
FROM
    orders o, customers c
WHERE
    o.customer_id = c.customer_id;
```

* Tables are listed separated by commas.
* Join condition is written in the `WHERE` clause.
* Functionally the same as an explicit `INNER JOIN`.

---

## 3. The Problem with Implicit Joins

If you **forget the `WHERE` clause**, you accidentally create a **CROSS JOIN**:

```sql
SELECT 
    *
FROM
    orders o, customers c;
```

* Produces a **Cartesian product**:

  * Every row from `orders` × every row from `customers`.
  * Example:

    * `orders` has 10 rows.
    * `customers` has 10 rows.
    * Result = **100 rows**.
* ❌ Usually not what you want.

---

## 4. Cross Join (Cartesian Product)

* A **cross join** returns **all possible combinations** of rows between two tables.
* Explicit syntax exists, but with implicit joins, it’s easy to trigger it **by mistake**.

---

## 5. Best Practice

* ✅ Always use **explicit `JOIN ... ON ...` syntax**.
* Avoid implicit joins:

  * They mix filtering conditions with join logic in the `WHERE` clause.
  * Harder to read and maintain.
  * Risk of **accidentally creating a cross join**.

---

## 6. Key Takeaways

* **Explicit Join Syntax**:

  ```sql
  SELECT ...
  FROM table1
  JOIN table2 ON condition;
  ```

  → Safe, clear, recommended.
* **Implicit Join Syntax**:

  ```sql
  SELECT ...
  FROM table1, table2
  WHERE condition;
  ```

  → Supported, but risky and outdated.
* Forgetting `WHERE` = **CROSS JOIN disaster**.

---

⚡ **Rule of thumb**: Use explicit joins for clarity and safety.

---

## Queries from the video

```sql
USE sql_store;

SELECT 
    *
FROM
    orders o
        JOIN
    customers c ON o.customer_id = c.customer_id;
    
-- Implicit join syntax
SELECT 
    *
FROM
    orders o,
    customers c
WHERE
    o.customer_id = c.customer_id;
    
-- Without WHERE clause we get Cross JOIN

SELECT 
    *
FROM
    orders o,
    customers c;
```