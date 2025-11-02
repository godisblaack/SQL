# 59 Calculating Dates and Times

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/pmdSeHkr9pg?si=aEnA8ck2X7U4jtjT"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🎯 0. Goal

Learn how to **add, subtract, and calculate differences** between date and time values using MySQL’s built-in functions.

---

## 🧱 1. Adding or Subtracting from Dates

### 🧩 `DATE_ADD()` Function

Used to **add a specific time interval** (like days, months, or years) to a given date.

### 🧠 Syntax:

```sql
DATE_ADD(date_value, INTERVAL n unit)
```

* `n`: the number to add
* `unit`: specifies what part to add (e.g., DAY, MONTH, YEAR, HOUR, MINUTE)

### 🧪 Examples:

```sql
SELECT DATE_ADD(NOW(), INTERVAL 1 DAY);   -- Adds 1 day (tomorrow)
SELECT DATE_ADD(NOW(), INTERVAL 1 YEAR);  -- Adds 1 year
```

✅ **Result Example:**

| Expression                         | Result              |
| ---------------------------------- | ------------------- |
| `DATE_ADD(NOW(), INTERVAL 1 DAY)`  | 2025-10-20 14:25:10 |
| `DATE_ADD(NOW(), INTERVAL 1 YEAR)` | 2026-10-19 14:25:10 |

---

## ⏪ 2. Going Back in Time

### 🧩 Option 1: Use Negative Values

```sql
SELECT DATE_ADD(NOW(), INTERVAL -1 YEAR);  -- Subtract 1 year
```

### 🧩 Option 2: Use `DATE_SUB()`

```sql
SELECT DATE_SUB(NOW(), INTERVAL 1 YEAR);   -- Same result as above
```

✅ **Result Example:**

| Expression                          | Result     |
| ----------------------------------- | ---------- |
| `DATE_ADD(NOW(), INTERVAL -1 YEAR)` | 2024-10-19 |
| `DATE_SUB(NOW(), INTERVAL 1 YEAR)`  | 2024-10-19 |

> 💡 **Tip:**
> Both approaches work identically — `DATE_SUB()` just makes intent clearer when subtracting.

---

## 📏 3. Calculating Date Differences

### 🧩 `DATEDIFF()` Function

Returns the **difference in days** between two dates.

### 🧠 Syntax:

```sql
DATEDIFF(date1, date2)
```

* Returns: `date1 - date2`
* Only measures **days** (ignores hours/minutes/seconds)

### 🧪 Examples:

```sql
SELECT DATEDIFF('2019-01-05', '2019-01-01');   -- 4 days
SELECT DATEDIFF('2019-01-05 09:00', '2019-01-01 17:00'); -- 4 days
SELECT DATEDIFF('2019-01-01 17:00', '2019-01-05 09:00'); -- -4 days
```

✅ **Result Example:**

| Expression                    | Result |
| ----------------------------- | ------ |
| `'2019-01-05' - '2019-01-01'` | 4      |
| `'2019-01-01' - '2019-01-05'` | -4     |

> ⚠️ **Note:**
> Even when you include time components (hours/minutes), `DATEDIFF()` still only returns **whole days**.

---

## ⏱️ 4. Calculating Time Differences

### 🧩 `TIME_TO_SEC()` Function

Converts a **time value** into the number of **seconds since midnight**.
Then you can subtract one from another to find the difference in seconds.

### 🧠 Syntax:

```sql
TIME_TO_SEC(time_value)
```

### 🧪 Example:

```sql
SELECT TIME_TO_SEC('09:00');             -- 32400 seconds
SELECT TIME_TO_SEC('09:00') - TIME_TO_SEC('09:02'); -- -120
SELECT TIME_TO_SEC('09:02') - TIME_TO_SEC('09:00'); -- 120
```

✅ **Result Example:**

| Expression | Result      |
| ---------- | ----------- |
| `'09:00'`  | 32400       |
| `'09:02'`  | 32520       |
| Difference | 120 seconds |

> 🧭 Positive or negative depends on **which time you subtract first**.

---

## 🧠 5. Key Takeaways

| Function        | Purpose                                 | Returns           |
| --------------- | --------------------------------------- | ----------------- |
| `DATE_ADD()`    | Add a time interval to a date           | Date/Datetime     |
| `DATE_SUB()`    | Subtract a time interval from a date    | Date/Datetime     |
| `DATEDIFF()`    | Difference between two dates (in days)  | Integer           |
| `TIME_TO_SEC()` | Converts time to seconds for comparison | Integer (seconds) |

---

## 6. Queries from the video

```SQL
SELECT
	DATE_ADD(NOW(), INTERVAL 1 DAY),
    DATE_ADD(NOW(), INTERVAL 1 YEAR),
    DATE_ADD(NOW(), INTERVAL -1 YEAR),
    DATE_SUB(NOW(), INTERVAL 1 YEAR),
    DATEDIFF('2019-01-05', '2019-01-01'),
    DATEDIFF('2019-01-05 09:00', '2019-01-01 17:00'),
    DATEDIFF('2019-01-01 17:00', '2019-01-05 09:00'),
    TIME_TO_SEC('09:00') - TIME_TO_SEC('09:02');
```