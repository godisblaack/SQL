# 46 Subqueries

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/mE27ZTCq7pk?si=TRRmTUhPygfipX72"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🧩 1. What is a Subquery?

A **subquery** is a secondary query enclosed in **parentheses (`( )`)** that provides data to the **outer query**.

It helps you perform operations that require a comparison or calculation based on other data from the same or another table.

---

## ⚙️ 2. Basic Syntax

```sql
SELECT 
    column_list
FROM 
    table_name
WHERE 
    column_name operator (
        SELECT column_name
        FROM another_table
        WHERE condition
    );
```

### 💡 Notes:

* The **inner query** (inside parentheses) runs **first**.
* Its result is passed to the **outer query**.
* Subqueries are often used with **comparison operators** like `=`, `>`, `<`, `IN`, `ANY`, and `ALL`.

---

## 🧩 3. Example — Products More Expensive than “Lettuce”

Let’s find all products in the `sql_store` database that are **more expensive than lettuce**.

```sql
USE sql_store;

SELECT 
    *
FROM
    products
WHERE
    unit_price > (
        SELECT 
            unit_price
        FROM
            products
        WHERE
            product_id = 3
    );
```

### 🔍 Explanation:

* The **inner query** fetches the `unit_price` of the product whose `product_id = 3` (Lettuce).
* The **outer query** then selects all products whose `unit_price` is **greater** than that value.
* This approach avoids hardcoding numbers like `WHERE unit_price > 3.50`.

---

## 🧠 4. How MySQL Executes a Subquery

1. MySQL first evaluates the **subquery** (the inner part).
2. It then substitutes the result into the **outer query**.
3. The outer query runs using that value.

In our example:

1. The subquery returns the price of “Lettuce.”
2. The outer query compares all products’ prices to that result.

---

## 🧱 5. Types of Subqueries

| Clause Used In | Description                           | Example Use Case                           |
| -------------- | ------------------------------------- | ------------------------------------------ |
| `WHERE`        | Filter results based on another query | Find products more expensive than X        |
| `FROM`         | Create a temporary derived table      | Compute grouped summaries before filtering |
| `SELECT`       | Return computed values per row        | Include an average or count inline         |

👉 In this lesson, we used a **subquery in the `WHERE` clause**, but we’ll explore the others later.

---

## 🧩 6. Exercise — Employees Earning More Than Average

In the `sql_hr` database, list all employees who earn **more than the average salary**.

```sql
USE sql_hr;

SELECT
    *
FROM
    employees
WHERE salary > (
    SELECT
        AVG(salary)
    FROM
        employees
);
```

### 🔍 Explanation:

* The subquery `SELECT AVG(salary) FROM employees` calculates the **average salary**.
* The outer query retrieves all employees whose salary is **greater than that average**.
* This makes the query **dynamic** — it automatically adjusts if salary data changes.

---

## ⚙️ 7. Key Rules and Notes

1. Subqueries must be **enclosed in parentheses**.
2. The **inner query** runs first.
3. Subqueries can return:

   * A **single value** (scalar subquery)
   * A **list of values** (used with `IN`)
   * A **table** (used in `FROM`)
4. You can **nest multiple subqueries**, but keep performance in mind.
5. Always ensure your subquery returns the correct **data type** for comparison.

---

## 💡 8. Key Takeaways

| Concept             | Description                                  |
| ------------------- | -------------------------------------------- |
| **Subquery**        | A query inside another query                 |
| **Execution order** | Inner query executes before the outer query  |
| **Common location** | `WHERE`, `FROM`, or `SELECT` clause          |
| **Use case**        | Compare data dynamically between tables      |
| **Performance tip** | Avoid unnecessary nesting for large datasets |

---

## 9. Queries from the video

```sql
USE sql_store;

SELECT 
    *
FROM
    products
WHERE
    unit_price > (
		SELECT 
            unit_price
        FROM
            products
        WHERE
            product_id = 3
	);
```

## 10. Exercise

### In sql_hr database, find employees whose earn more than average

```sql
USE sql_hr;

SELECT
	*
FROM
	employees
WHERE salary > (
	SELECT
		AVG(salary)
	FROM
		employees
	);
```