# 55 Numeric Functions

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/hlYJc3drHV0?si=GeOddGViJp6afv1X"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 🎯 1. Goal

Learn how to manipulate **numeric values** in MySQL using built-in functions such as rounding, truncation, and absolute values.

---

## ⚙️ 2. Functions Overview

| Function           | Description                                                    | Example               | Result         |
| ------------------ | -------------------------------------------------------------- | --------------------- | -------------- |
| **ROUND(x [, d])** | Rounds a number `x` to `d` decimal places (default = 0).       | `ROUND(5.7345, 2)`    | `5.73`         |
| **TRUNCATE(x, d)** | Truncates a number `x` to `d` decimal places (does not round). | `TRUNCATE(5.7345, 2)` | `5.73`         |
| **CEILING(x)**     | Returns the **smallest integer ≥ x**.                          | `CEILING(5.2)`        | `6`            |
| **FLOOR(x)**       | Returns the **largest integer ≤ x**.                           | `FLOOR(5.2)`          | `5`            |
| **ABS(x)**         | Returns the **absolute value** (always positive).              | `ABS(-5.2)`           | `5.2`          |
| **RAND()**         | Returns a **random number** between `0` and `1`.               | `RAND()`              | e.g. `0.48692` |

---

## 💻 3. Example Query

```sql
SELECT ROUND(5.7345, 2)
UNION ALL
SELECT TRUNCATE(5.7345, 2)
UNION ALL
SELECT CEILING(5.2)
UNION ALL
SELECT FLOOR(5.2)
UNION ALL
SELECT ABS(-5.2)
UNION ALL
SELECT RAND();
```

### ✅ Output (sample)

| result  |
| ------- |
| 5.73    |
| 6       |
| 5       |
| 5.2     |
| 0.48692 |

---

## 🔍 4. Explanation

* **`ROUND`** — rounds based on standard math rules (≥ .5 → up).
* **`TRUNCATE`** — simply **cuts off digits**, no rounding.
* **`CEILING`** — always rounds **up** to the nearest integer.
* **`FLOOR`** — always rounds **down**.
* **`ABS`** — removes the sign from a number.
* **`RAND`** — useful for generating random values, testing, or sampling.

---

## 💡 5. Key Takeaways

| Tip                       | Description                                                        |
| ------------------------- | ------------------------------------------------------------------ |
| ✅ `ROUND()`               | Use for mathematical rounding.                                     |
| ✅ `TRUNCATE()`            | Use when you want to **cut digits**, not round.                    |
| ✅ `CEILING()` / `FLOOR()` | Convert decimals to whole numbers consistently up/down.            |
| ✅ `ABS()`                 | Always returns positive values — great for dealing with negatives. |
| ✅ `RAND()`                | Generates pseudorandom decimals in `[0, 1)`.                       |

---

## 🔗 6. Explore More

For the full list of numeric functions, check the official docs:
🔗 [MySQL Numeric Functions — Dev Docs](https://dev.mysql.com/doc/refman/en/numeric-functions.html)

---

## Queries from the video

```SQL
SELECT ROUND(5.7345, 2)
UNION ALL
SELECT TRUNCATE(5.7345, 2)
UNION ALL
SELECT CEILING(5.2)
UNION ALL
SELECT FLOOR(5.2)
UNION ALL
SELECT ABS(-5.2)
UNION ALL
SELECT RAND();
```