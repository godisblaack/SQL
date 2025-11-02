# 61 The IF Function

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/CD08klJL52I?si=BCaMe94bsFPKw4LO"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## ⚖️ 0. Conditional Expressions in MySQL — `IF()` Function

Learn how to **evaluate conditions** and **return different values** based on whether a condition is true or false — directly within a SQL query.

---

## 🧩 1. The `IF()` Function

### 🧠 Syntax:

```sql
IF(condition, value_if_true, value_if_false)
```

* `condition`: the logical test (e.g., `x > 10`)
* `value_if_true`: returned if the condition is **true**
* `value_if_false`: returned if the condition is **false**

✅ Think of it as SQL’s version of an **“if-else” statement**.

---

## 🧪 2. Example 1 — Classify Orders as *Active* or *Archived*

### 🧠 Goal:

Label each order as:

* **Active** → if placed in the current year
* **Archived** → otherwise

### 💻 Query:

```sql
USE sql_store;

SELECT
	order_id,
	order_date,
	IF(
		YEAR(order_date) = YEAR(NOW()),
		'Active',
		'Archived'
	) AS category
FROM
	orders;
```

✅ **Output Example:**

| order_id | order_date | category |
| -------- | ---------- | -------- |
| 1        | 2019-01-06 | Active   |
| 2        | 2018-11-10 | Archived |
| 3        | 2019-03-02 | Active   |

> 💬 Here, `YEAR(order_date) = YEAR(NOW())` tests whether the order’s year equals the current year.

---

## 🔍 3. Explanation

| Part                    | Description                                |
| ----------------------- | ------------------------------------------ |
| `IF()`                  | Conditional function                       |
| `YEAR(order_date)`      | Extracts the year from order date          |
| `YEAR(NOW())`           | Gets the current year                      |
| `'Active' / 'Archived'` | Values returned depending on the condition |

> 💡 You can return **any type** — string, number, date, or even `NULL`.

---

## 🧠 4. Exercise — Classify Products by Order Frequency

### 🧠 Goal:

Show how many times each product has been ordered, and label it as:

* **“Many times”** if ordered more than once
* **“Once”** if ordered only once

---

### 💻 Solution:

```sql
USE sql_store;

SELECT
	product_id,
	name,
	COUNT(*) AS orders,
	IF(
		COUNT(*) > 1,
		'Many times',
		'Once'
	) AS frequency
FROM
	products
		JOIN order_items
		USING (product_id)
GROUP BY
	product_id, name;
```

✅ **Output Example:**

| product_id | name                       | orders | frequency  |
| ---------- | -------------------------- | ------ | ---------- |
| 1          | Foam Dinner Plate          | 3      | Many times |
| 2          | Pork – Bacon, back Peameal | 2      | Many times |
| 8          | Island Oasis – Raspberry   | 1      | Once       |
| 10         | Broom – Push               | 1      | Once       |

---

## 🧠 5. Key Takeaways

| Function                                         | Description                                                                               | Returns                  |
| ------------------------------------------------ | ----------------------------------------------------------------------------------------- | ------------------------ |
| `IF(condition, true_val, false_val)`             | Tests a condition and returns one of two values                                           | Value based on condition |
| `IF()` vs `IFNULL()`                             | `IFNULL()` only checks if something is `NULL`, `IF()` can check **any logical condition** |                          |
| Works inside `SELECT`, `WHERE`, `ORDER BY`, etc. | Adds flexible logic directly into queries                                                 |                          |

---

## 🧩 6. Optional: Using `CASE` for More Complex Conditions

> When you need **multiple conditions**, use `CASE` instead of nested `IF()`.

Example:

```sql
CASE
	WHEN COUNT(*) > 10 THEN 'Very popular'
	WHEN COUNT(*) > 1 THEN 'Popular'
	ELSE 'Rare'
END AS frequency
```

---

## 7. Queries from the video

```sql
USE sql_store;

SELECT
	order_id,
    order_date,
    IF(
		YEAR(order_date) = YEAR(NOW()), 
		'Active', 
        'Archived'
	) AS category
FROM
	orders;
```

## 8. Exercise

### Generate the following table from sql_store database.

|product_id	| name							| orders	| frequency |
|-----------|-------------------------------|-----------|-----------|
|1			| Foam Dinner Plate				| 3		    | Many times|
|2			| Pork - Bacon,back Peameal		| 2		    | Many times|
|3			| Lettuce - Romaine, Heart		| 4		    | Many times|
|4			| Brocolinni - Gaylan, Chinese	| 2		    | Many times|
|5			| Sauce - Ranch Dressing		| 2		    | Many times|
|6			| Petit Baguette				| 2		    | Many times|
|8			| Island Oasis - Raspberry		| 1		    | Once      |
|9			| Longan						| 1		    | Once      |
|10			| Broom - Push					| 1		    | Once      |

```sql
USE sql_store;

SELECT
	orderCount.product_id,
    p.name,
    orderCount.orders,
    IF (
		orderCount.orders > 1,
        'Many times',
        'Once'
	) AS frequency
FROM (
	SELECT
		product_id,
		COUNT(product_id) AS orders
	FROM
		order_items
	GROUP BY
		product_id
) AS orderCount
		JOIN
	products p
		ON
	orderCount.product_id = p.product_id;
    
-- Solution by mosh

SELECT
	product_id,
    name,
    COUNT(*) AS orders,
    IF (
		COUNT(*) > 1,
        'Many times',
        'Once'
	) AS frequency
FROM
	products
		JOIN
	order_items
		USING
    (product_id)
GROUP BY
	product_id,
    name;
```