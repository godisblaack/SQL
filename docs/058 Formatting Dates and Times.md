# 58 Formatting Dates and Times

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/LnbM9C5iAeo?si=xiDP3FuR4TBAqCG7"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🎯 1. Goal

Learn how to **display dates and times in a user-friendly format** instead of the default developer-friendly `YYYY-MM-DD` structure.

---

## 🧱 2. Default Date Format

* MySQL stores dates as:

  ```
  YYYY-MM-DD
  ```

  👉 Example: `2019-03-11`

* While this is ideal for **computers and developers**, it’s not always ideal for **end users** who might prefer something like:

  ```
  March 11, 2019
  ```

---

## 🧩 3. The `DATE_FORMAT()` Function

Used to **format a date value** into a custom string using *format specifiers*.

### 🧠 Syntax:

```sql
DATE_FORMAT(date_value, format_string)
```

### 🧰 Common Format Specifiers

| Code | Meaning                  | Example (2025-10-19) |
| ---- | ------------------------ | -------------------- |
| `%Y` | 4-digit year             | 2025                 |
| `%y` | 2-digit year             | 25                   |
| `%M` | Full month name          | October              |
| `%m` | 2-digit month            | 10                   |
| `%D` | Day of month with suffix | 19th                 |
| `%d` | 2-digit day of month     | 19                   |

### 🧪 Examples

```sql
SELECT
    DATE_FORMAT(NOW(), '%y') AS short_year,       -- 25
    DATE_FORMAT(NOW(), '%Y') AS full_year,        -- 2025
    DATE_FORMAT(NOW(), '%m') AS month_number,     -- 10
    DATE_FORMAT(NOW(), '%M') AS month_name,       -- October
    DATE_FORMAT(NOW(), '%M %d %Y') AS full_date;  -- October 19 2025
```

✅ **Result Example:**

| short_year | full_year | month_number | month_name | full_date       |
| ---------- | --------- | ------------ | ---------- | --------------- |
| 25         | 2025      | 10           | October    | October 19 2025 |

---

## ⏰ 4. The `TIME_FORMAT()` Function

Used to **format time values** into readable strings.

### 🧠 Syntax:

```sql
TIME_FORMAT(time_value, format_string)
```

### 🧰 Common Format Specifiers

| Code | Meaning         | Example |
| ---- | --------------- | ------- |
| `%H` | Hour (00–23)    | 15      |
| `%h` | Hour (01–12)    | 03      |
| `%i` | Minutes (00–59) | 34      |
| `%s` | Seconds (00–59) | 45      |
| `%p` | AM or PM        | PM      |

### 🧪 Examples

```sql
SELECT
    TIME_FORMAT(NOW(), '%H') AS hour_24,
    TIME_FORMAT(NOW(), '%i') AS minutes,
    TIME_FORMAT(NOW(), '%p') AS meridian,
    TIME_FORMAT(NOW(), '%H:%i %p') AS formatted_time;
```

✅ **Result Example:**

| hour_24 | minutes | meridian | formatted_time |
| ------- | ------- | -------- | -------------- |
| 15      | 34      | PM       | 15:34 PM       |

---

## 🔗 5. Reference Table

📘 For a full list of all format specifiers, visit:
👉 [MySQL DATE_FORMAT() Function Reference](https://dev.mysql.com/doc/refman/en/date-and-time-functions.html#function_date-format)

---

## 🧠 6. Key Takeaways

| Concept              | Summary                                                   |
| -------------------- | --------------------------------------------------------- |
| 🗓️ `DATE_FORMAT()`  | Converts dates into custom display formats.               |
| ⏰ `TIME_FORMAT()`    | Converts times into user-friendly strings.                |
| 💡 Format specifiers | Codes starting with `%` to control output.                |
| 🧭 Real-world use    | Perfect for reports, dashboards, or formatted UI queries. |

---

## 7. Queries from the video

```SQL
SELECT
	DATE_FORMAT(NOW(), '%y'),
    DATE_FORMAT(NOW(), '%Y'),
    DATE_FORMAT(NOW(), '%m'),
    DATE_FORMAT(NOW(), '%M'),
    DATE_FORMAT(NOW(), '%M %d %Y'),
    TIME_FORMAT(NOW(), '%H'),
    TIME_FORMAT(NOW(), '%i'),
    TIME_FORMAT(NOW(), '%p'),
    TIME_FORMAT(NOW(), '%H:%i %p');
```