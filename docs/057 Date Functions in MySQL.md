# 57 Date Functions in MySQL

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/BYhV4gbAxK8?si=0jurSyJHKAu-eZ4n"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🎯 1. Overview

Learn how to:

* Retrieve the current date/time
* Extract specific components (year, month, day, etc.)
* Write dynamic, time-aware queries (e.g. “orders placed this year”)

---

## ⚙️ 2. Core Date & Time Functions

| Function                           | Description                    | Example     | Result (sample)       |
| ---------------------------------- | ------------------------------ | ----------- | --------------------- |
| **NOW()**                          | Returns current date and time. | `NOW()`     | `2025-10-19 15:34:10` |
| **CURDATE()** / **CURRENT_DATE()** | Returns current date only.     | `CURDATE()` | `2025-10-19`          |
| **CURTIME()** / **CURRENT_TIME()** | Returns current time only.     | `CURTIME()` | `15:34:10`            |

---

## 🧩 3. Extracting Components

| Function            | Description                     | Example            | Result    |
| ------------------- | ------------------------------- | ------------------ | --------- |
| **YEAR(date)**      | Extracts the year.              | `YEAR(NOW())`      | `2025`    |
| **MONTH(date)**     | Extracts the month (as number). | `MONTH(NOW())`     | `10`      |
| **DAY(date)**       | Extracts the day of the month.  | `DAY(NOW())`       | `19`      |
| **HOUR(date)**      | Extracts the hour.              | `HOUR(NOW())`      | `15`      |
| **MINUTE(date)**    | Extracts the minute.            | `MINUTE(NOW())`    | `34`      |
| **SECOND(date)**    | Extracts the second.            | `SECOND(NOW())`    | `10`      |
| **DAYNAME(date)**   | Returns day name as text.       | `DAYNAME(NOW())`   | `Sunday`  |
| **MONTHNAME(date)** | Returns month name as text.     | `MONTHNAME(NOW())` | `October` |

---

## 🧱 4. Standard SQL Alternative

The **EXTRACT()** function is part of the **SQL standard** (portable to other RDBMS like PostgreSQL or Oracle):

```sql
EXTRACT(YEAR FROM NOW());
EXTRACT(DAY FROM NOW());
```

| Example                    | Result |
| -------------------------- | ------ |
| `EXTRACT(YEAR FROM NOW())` | `2025` |
| `EXTRACT(DAY FROM NOW())`  | `19`   |

---

## 🧮 5. Example Query

```sql
SELECT
	NOW() AS current_datetime,
	CURDATE() AS current_date,
	CURTIME() AS current_time,
	YEAR(NOW()) AS current_year,
	MONTH(NOW()) AS current_month,
	DAY(NOW()) AS current_day,
	HOUR(NOW()) AS current_hour,
	MINUTE(NOW()) AS current_minute,
	SECOND(NOW()) AS current_second,
	DAYNAME(NOW()) AS weekday_name,
	MONTHNAME(NOW()) AS month_name,
	EXTRACT(DAY FROM NOW()) AS extracted_day,
	EXTRACT(YEAR FROM NOW()) AS extracted_year;
```

✅ **Result example:**

| current_datetime    | current_date | current_time | current_year | weekday_name | month_name |
| ------------------- | ------------ | ------------ | ------------ | ------------ | ---------- |
| 2025-10-19 15:34:10 | 2025-10-19   | 15:34:10     | 2025         | Sunday       | October    |

---

## 🧠 6. Exercise Solution

### ❌ Original (hard-coded year):

```sql
USE sql_store;

SELECT *
FROM orders
WHERE order_date >= '2019-01-01';
```

⚠️ Problem: This only works for **2019** and will break in future years.

---

### ✅ Correct Dynamic Version:

```sql
USE sql_store;

SELECT
	*
FROM
	orders
WHERE
	YEAR(order_date) = YEAR(NOW());
```

🧩 **Explanation:**

* `YEAR(NOW())` → current year from system clock
* `YEAR(order_date)` → year of each order
* Compare both to get only current-year orders.

---

## 💡 7. Bonus Tips

| Tip                                                                                                              | Description |
| ---------------------------------------------------------------------------------------------------------------- | ----------- |
| 🧭 Use `CURDATE()` instead of hardcoded dates to make queries future-proof.                                      |             |
| 📆 You can combine with arithmetic: `DATE_ADD(CURDATE(), INTERVAL 7 DAY)` or `DATE_SUB(NOW(), INTERVAL 1 MONTH)` |             |
| 🧱 Use `EXTRACT()` when writing SQL meant to run on multiple database systems.                                   |             |

---

## 🔗 8. Explore More

📘 [MySQL Date and Time Functions (Official Docs)](https://dev.mysql.com/doc/refman/en/date-and-time-functions.html)

---

## 9. Queries from the video

```sql
SELECT
	NOW(),
    CURDATE(),
    CURTIME(),
    YEAR(NOW()),
    MONTH(NOW()),
    DAY(NOW()),
    HOUR(NOW()),
    MINUTE(NOW()),
    SECOND(NOW()),
    DAYNAME(NOW()),
    MONTHNAME(NOW()),
    EXTRACT(DAY FROM NOW()),
    EXTRACT(YEAR FROM NOW());
```

## 10. Exercise

### Modify the following SQL query to return the order placed in the current year.

```sql
USE sql_store;

SELECT
	*
FROM
	orders
WHERE
	order_date >= '2019-01-01';
```

```sql
USE sql_store;

SELECT
	*
FROM
	orders
WHERE
	YEAR(order_date) = YEAR(NOW());
```