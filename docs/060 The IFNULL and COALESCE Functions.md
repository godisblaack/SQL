# 60 The IFNULL and COALESCE Functions

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/sXTAAwKxJu4?si=xNQFyMg6sqIpBub2"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🧩 0. Handling NULL Values in MySQL — `IFNULL()` & `COALESCE()`

Learn how to **replace or handle NULL values** gracefully when displaying or processing data in MySQL.

---

## 🧱 1. Understanding `NULL`

* `NULL` represents **missing or unknown data** in a database.
* When displayed to users, `NULL` is often confusing or visually unappealing.
* MySQL provides **functions to replace NULL values** with a default or fallback value.

---

## 🧩 2. The `IFNULL()` Function

### 🧠 Syntax:

```sql
IFNULL(expression, replacement_value)
```

* If `expression` is **not NULL**, returns its value.
* If `expression` **is NULL**, returns the `replacement_value`.

### 🧪 Example:

```sql
SELECT
	order_id,
	IFNULL(shipper_id, 'Not assigned') AS shipper
FROM
	orders;
```

✅ **Result Example:**

| order_id | shipper          |
| -------- | ---------------- |
| 1        | 3                |
| 2        | 1                |
| 3        | **Not assigned** |
| 4        | 2                |
| 5        | **Not assigned** |

> 💡 Use `IFNULL()` when you only need to substitute **one** possible NULL value.

---

## 🧩 3. The `COALESCE()` Function

### 🧠 Syntax:

```sql
COALESCE(value1, value2, value3, ...)
```

* Returns the **first non-NULL value** in the list.
* Useful for **multiple fallback options**.

### 🧪 Example:

```sql
SELECT
	order_id,
	COALESCE(shipper_id, comments, 'Not assigned') AS shipper
FROM
	orders;
```

✅ **Result Example:**

| order_id | shipper                             |
| -------- | ----------------------------------- |
| 6        | 2                                   |
| 8        | “Urgent delivery” *(from comments)* |
| 10       | **Not assigned**                    |

> 💬 **Difference from `IFNULL()`**
>
> * `IFNULL()` → handles **only one** potential NULL
> * `COALESCE()` → checks **multiple values** and returns the first valid one

---

## 🧩 4. Exercise — Display Customer Contact Info

### 🧠 Problem:

Display customers’ **full names** and **phone numbers**, but if a phone number is missing, show “Unknown”.

### 🧪 Solution:

```sql
USE sql_store;

SELECT
	CONCAT(first_name, ' ', last_name) AS customer,
	IFNULL(phone, 'Unknown') AS phone
	-- OR: COALESCE(phone, 'Unknown') AS phone
FROM
	customers;
```

✅ **Result Example:**

| customer          | phone        |
| ----------------- | ------------ |
| Babara MacCaffrey | 781-932-9754 |
| Ines Brushfield   | 804-427-9456 |
| Clemmie Betchley  | **Unknown**  |
| Romola Rumgay     | 559-181-3744 |

---

## 🧠 5. Key Takeaways

| Function                    | Description                                       | Returns         |
| --------------------------- | ------------------------------------------------- | --------------- |
| `IFNULL(expr, replacement)` | Replaces a NULL with a given value                | Single fallback |
| `COALESCE(v1, v2, v3, …)`   | Returns the first non-NULL among multiple options | First non-NULL  |

> 🧭 **Tip:**
> `COALESCE()` is part of the **standard SQL** specification — it’s more portable across different database systems.

---

## 6. Queries from the video

```sql
USE sql_store;

SELECT
	*
FROM
	orders;
    
SELECT
	order_id,
    IFNULL(shipper_id, 'Not assigned') AS shipper
FROM
	orders;
    
    SELECT
	order_id,
    COALESCE(shipper_id, comments, 'Not assigned') AS shipper
FROM
	orders;
```

## 7. Exercise

### Generate the following table from customers table in sql_store database.

| customer			| phone        |
|-------------------|--------------|
| Babara MacCaffrey	| 781-932-9754 |
| Ines Brushfield	| 804-427-9456 |
| Freddi Boagey		| 719-724-7869 |
| Ambur Roseburgh	| 407-231-8017 |
| Clemmie Betchley	| Unknown      |
| Elka Twiddell		| 312-480-8498 |
| Ilene Dowson		| 615-641-4759 |
| Thacher Naseby	| 941-527-3977 |
| Romola Rumgay		| 559-181-3744 |
| Levy Mynett		| 404-246-3370 |


```sql
USE sql_store;

SELECT
	CONCAT(first_name, ' ', last_name) AS customer,
    IFNULL(phone, 'Unknown') AS phone
    -- COALESCE(phone, 'Unknown') AS phone
FROM
	customers;
```