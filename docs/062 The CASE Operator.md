# 62 The CASE Operator

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/z51gr-qI7xk?si=ap67px_QZH41eL5Y"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🧮 0. Conditional Logic with `CASE` Operator in MySQL

Learn how to test **multiple conditions** and return different values for each — all within a single query.

---

## 🧩 1. Why `CASE`?

* The `IF()` function only supports **one condition** (true/false).
* When you have **more than two possible outcomes**, use the **`CASE`** operator.

✅ Think of `CASE` as SQL’s version of **“if / else if / else”**.

---

## 🧠 2. Syntax

```sql
CASE
	WHEN condition1 THEN result1
	WHEN condition2 THEN result2
	WHEN condition3 THEN result3
	ELSE default_result
END
```

> Each `WHEN` is a test.
> The first condition that evaluates to **TRUE** determines the output.
> If no condition is true, the `ELSE` value is returned.

---

## 🧪 3. Example 1 — Categorize Orders by Date

### 🧠 Goal:

Classify each order as:

* **Active** → Placed this year
* **Last Year** → Placed last year
* **Archived** → Placed two or more years ago
* **Future** → Placed in a future year (optional)

---

### 💻 Query:

```sql
USE sql_store;

SELECT
	order_id,
	order_date,
	CASE
		WHEN YEAR(order_date) = YEAR(NOW()) 
			THEN 'Active'
		WHEN YEAR(order_date) = YEAR(NOW()) - 1
			THEN 'Last Year'
		WHEN YEAR(order_date) < YEAR(NOW()) - 1
			THEN 'Archived'
		ELSE 'Future'
	END AS category
FROM
	orders;
```

✅ **Output Example:**

| order_id | order_date | category  |
| -------- | ---------- | --------- |
| 1        | 2019-02-10 | Active    |
| 2        | 2018-05-12 | Last Year |
| 3        | 2017-03-01 | Archived  |

---

## 🧠 4. Explanation

| Part          | Meaning                                              |
| ------------- | ---------------------------------------------------- |
| `CASE`        | Begins the conditional block                         |
| `WHEN`        | Tests a specific condition                           |
| `THEN`        | Value returned if the condition is true              |
| `ELSE`        | Optional fallback if none of the conditions are true |
| `END`         | Closes the `CASE` block                              |
| `AS category` | Renames the resulting column                         |

---

## 🧩 5. Example 2 — Categorize Customers by Points

### 🧠 Goal:

Label customers based on their **loyalty points**:

* **Gold** → More than 3,000 points
* **Silver** → Between 2,000 and 3,000 (inclusive)
* **Bronze** → Less than 2,000 points

---

### 💻 Query:

```sql
USE sql_store;

SELECT
	CONCAT(first_name, ' ', last_name) AS customer,
	points,
	CASE
		WHEN points > 3000 THEN 'Gold'
		WHEN points >= 2000 THEN 'Silver'
		ELSE 'Bronze'
	END AS category
FROM
	customers
ORDER BY
	points DESC;
```

✅ **Output Example:**

| customer          | points | category |
| ----------------- | ------ | -------- |
| Babara MacCaffrey | 3970   | Gold     |
| Elka Twiddell     | 2650   | Silver   |
| Clemmie Betchley  | 1450   | Bronze   |

---

## 🧩 6. `CASE` vs `IF()`

| Feature                      | `IF()`         | `CASE`            |
| ---------------------------- | -------------- | ----------------- |
| Supports multiple conditions | ❌ No           | ✅ Yes             |
| Syntax readability           | Simple         | More structured   |
| Output type                  | Single value   | Multiple outcomes |
| SQL standard                 | MySQL-specific | ✅ Standard SQL    |

✅ Use `IF()` for **simple binary logic**
✅ Use `CASE` for **multiple logical branches**

---

## 🧠 7. Key Takeaways

* `CASE` allows **multiple test expressions** in a clean, readable way.
* Only the **first matching `WHEN`** is executed.
* Always end the block with `END`.
* `ELSE` is optional but recommended to catch all unmatched cases.

---

## 8. Queries from the video

```sql
USE sql_store;

SELECT
	order_id,
    order_date,
    CASE
		WHEN YEAR(order_date) = YEAR(NOW()) 
        THEN 'Active'
        WHEN YEAR(order_date) = YEAR(NOW() - 1) 
        THEN 'Last Year'
        WHEN YEAR(order_date) = YEAR(NOW()) 
        THEN 'Archived'
        ELSE 'Future'
	END AS category
FROM
	orders;
```

## 9. Exercise

### Categorize the customer with 'Gold' who has more than 3000 points, 'Silver' who has more than or equal to 2000 points, and 'Bronze' otherwise.

```sql
USE sql_store;

SELECT
	CONCAT(first_name, ' ', last_name) AS customer,
    points,
    CASE
		WHEN points > 3000
        THEN 'Gold'
        WHEN points >= 2000
        THEN 'Silver'
        ELSE 'Bronze'
	END AS category
FROM
	customers
ORDER BY
	points DESC;
```