# 41 Aggregate Functions

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/yg6OaBAdifY?si=NEsL_XAH-VtEE4q5"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 📘 1. Overview

In this section, we learn how to **summarize data** using SQL’s **aggregate functions** — essential for generating reports like **total sales by client, state, or time period**.

These are core SQL skills for analytics, business intelligence, and data reporting.

---

## ⚙️ 2. What Are Aggregate Functions?

An **aggregate function** performs a calculation on a set (or group) of values and returns a **single summarized value**.

### Common Aggregate Functions

| Function  | Description                   | Example                |
| --------- | ----------------------------- | ---------------------- |
| `MAX()`   | Returns the maximum value     | `MAX(invoice_total)`   |
| `MIN()`   | Returns the minimum value     | `MIN(invoice_total)`   |
| `AVG()`   | Returns the average of values | `AVG(invoice_total)`   |
| `SUM()`   | Returns the sum of all values | `SUM(invoice_total)`   |
| `COUNT()` | Returns the number of records | `COUNT(invoice_total)` |

---

## 🧩 3. Syntax Example

```sql
USE sql_invoicing;

SELECT 
    MAX(invoice_total) AS highest,
    MIN(invoice_total) AS lowest,
    AVG(invoice_total) AS average,
    SUM(invoice_total) AS total,
    COUNT(invoice_total) AS number_of_invoices
FROM
    invoices;
```

### 🔍 Output Explanation

| Column               | Meaning                          |
| -------------------- | -------------------------------- |
| `highest`            | Maximum invoice total            |
| `lowest`             | Minimum invoice total            |
| `average`            | Average of all invoices          |
| `total`              | Total of all invoice totals      |
| `number_of_invoices` | Total number of invoices counted |

---

## 🧠 4. Important Notes About Aggregate Functions

### 1. 🚫 **Null Values Are Ignored**

Aggregate functions (except `COUNT(*)`) **ignore NULL values**.

Example:

```sql
SELECT COUNT(payment_date) AS count_of_payments
FROM invoices;
```

→ Counts only invoices **with non-null payment dates**.

To count *all* rows:

```sql
SELECT COUNT(*) AS total_records
FROM invoices;
```

---

### 2. 🧮 **Using Expressions Inside Aggregates**

You can perform arithmetic expressions within aggregate functions.

Example:

```sql
SELECT 
    SUM(invoice_total * 1.1) AS total_with_tax
FROM
    invoices;
```

→ Multiplies each invoice by 1.1 (adding 10%) before summing.

---

### 3. 🧾 **Filtering Before Aggregation**

Use a `WHERE` clause to filter records *before* aggregation:

```sql
SELECT 
    SUM(invoice_total) AS total_sales
FROM
    invoices
WHERE
    invoice_date > '2019-07-01';
```

→ Calculates sales **only for invoices issued after July 1, 2019**.

---

### 4. 🧍‍♂️ **Counting Unique Values**

To count **unique** (distinct) values:

```sql
SELECT COUNT(DISTINCT client_id) AS unique_clients
FROM invoices
WHERE invoice_date > '2019-07-01';
```

→ Returns the number of **distinct clients** who had invoices after July 1, 2019.

---

## 🧾 5. Full Example Query

```sql
USE sql_invoicing;

SELECT 
    MAX(invoice_total) AS highest,
    MIN(invoice_total) AS lowest,
    AVG(invoice_total) AS average,
    SUM(invoice_total) AS total,
    SUM(invoice_total * 1.1) AS total_with_tax,
    COUNT(invoice_total) AS number_of_invoices,
    COUNT(payment_date) AS count_of_payments,
    COUNT(*) AS total_records,
    COUNT(DISTINCT client_id) AS distinct_total_records
FROM
    invoices
WHERE
    invoice_date > '2019-07-01';
```

### 🧩 6. Result Interpretation

| Column                   | Meaning                             |
| ------------------------ | ----------------------------------- |
| `highest`                | Largest invoice total               |
| `lowest`                 | Smallest invoice total              |
| `average`                | Mean invoice total                  |
| `total`                  | Overall total                       |
| `total_with_tax`         | Total with tax applied              |
| `number_of_invoices`     | Count of invoices (non-null totals) |
| `count_of_payments`      | Count of invoices with payments     |
| `total_records`          | Count of all rows                   |
| `distinct_total_records` | Count of unique clients             |

---

## 🧠 7. Using Aggregates with Text and Dates

You can use aggregate functions on **date** or **string** columns.

Example:

```sql
SELECT MAX(payment_date) AS last_payment_date
FROM invoices;
```

→ Returns the **most recent payment date**.

---

## 🧾 8. EXERCISE — Summarizing Half-Year Sales

### 🧍 Task

Generate a report showing total sales, total payments, and what we expect to receive for:

1. First half of 2019
2. Second half of 2019
3. Total of 2019

### 📊 9. Expected Output

| date_range          | total_sales | total_payments | what_we_expect |
| ------------------- | ----------- | -------------- | -------------- |
| First half of 2019  | 1539.07     | 662.69         | 876.38         |
| Second half of 2019 | 1051.53     | 355.02         | 696.51         |
| Total               | 2590.60     | 1017.71        | 1572.89        |


**Note:** Output will be different because the database provided by Mosh has different value, then what he has used in the video. He might have updated it later.

---

### ✅ 10. Solution

```sql
USE sql_invoicing;

SELECT 
    'First half of 2019' AS date_range,
    SUM(invoice_total) AS total_sales,
    SUM(payment_total) AS total_payments,
    SUM(invoice_total) - SUM(payment_total) AS what_we_expect
FROM
    invoices
WHERE
    invoice_date BETWEEN '2019-01-01' AND '2019-06-30'

UNION

SELECT 
    'Second half of 2019' AS date_range,
    SUM(invoice_total) AS total_sales,
    SUM(payment_total) AS total_payments,
    SUM(invoice_total) - SUM(payment_total) AS what_we_expect
FROM
    invoices
WHERE
    invoice_date BETWEEN '2019-07-01' AND '2019-12-31'

UNION

SELECT 
    'Total' AS date_range,
    SUM(invoice_total) AS total_sales,
    SUM(payment_total) AS total_payments,
    SUM(invoice_total) - SUM(payment_total) AS what_we_expect
FROM
    invoices
WHERE
	invoice_date BETWEEN '2019-01-01' AND '2019-12-31';
```

---

### 🧠 11. Explanation

* `UNION` combines multiple queries into one result.
* Each subquery summarizes a specific time range.
* The third query (`Total`) combines all 2019 data.
* `SUM(invoice_total) - SUM(payment_total)` gives the **expected payment** still due.

---

## ⚡ 12. Key Takeaways

* Aggregate functions summarize data across rows.
* They ignore `NULL` values (unless `COUNT(*)` is used).
* You can apply expressions inside aggregate functions.
* Always filter your data **before aggregation** with `WHERE`.
* Use `DISTINCT` inside aggregate functions to remove duplicates.
* Combine multiple result sets using `UNION`.

---


## 13. Queries from the video

```sql
USE sql_invoicing;

SELECT 
    MAX(invoice_total) AS highest,
    MAX(payment_date) AS highest,
    MIN(invoice_total) AS lowest,
    AVG(invoice_total) AS average,
    SUM(invoice_total) AS total,
    SUM(invoice_total * 1.1) AS total,
    COUNT(invoice_total) AS number_of_invoices,
    COUNT(payment_date) AS count_of_paryments,
    COUNT(*) AS total_records,
    COUNT(client_id) AS total_records,
    COUNT(DISTINCT client_id) AS distinct_total_records
FROM
    invoices
WHERE
    invoice_date > '2019-07-01';
```

## 14. Exercise

Generate a report showing total sales, total payments, and what we expect to receive for:

1. First half of 2019
2. Second half of 2019
3. Total of 2019

Expected Output

| date_range          | total_sales | total_payments | what_we_expect |
| ------------------- | ----------- | -------------- | -------------- |
| First half of 2019  | 1539.07     | 662.69         | 876.38         |
| Second half of 2019 | 1051.53     | 355.02         | 696.51         |
| Total               | 2590.60     | 1017.71        | 1572.89        |


**Note:** Output will be different because the database provided by Mosh has different value, then what he has used in the video. He might have updated it later.

```sql
USE sql_invoicing;

SELECT 
    'First half of 2019' AS date_range,
    SUM(invoice_total) AS total_sales,
    SUM(payment_total) AS total_payments,
    SUM(invoice_total) - SUM(payment_total) AS what_we_expect
FROM
    invoices
WHERE
    invoice_date BETWEEN '2019-01-01' AND '2019-06-30'
    
UNION

SELECT 
    'Second half of 2019' AS date_range,
    SUM(invoice_total) AS total_sales,
    SUM(payment_total) AS total_payments,
    SUM(invoice_total) - SUM(payment_total) AS what_we_expect
FROM
    invoices
WHERE
    invoice_date BETWEEN '2019-07-01' AND '2019-12-31'

UNION

SELECT 
    'Total' AS date_range,
    SUM(invoice_total) AS total_sales,
    SUM(payment_total) AS total_payments,
    SUM(invoice_total) - SUM(payment_total) AS what_we_expect
FROM
    invoices
WHERE
	invoice_date BETWEEN '2019-01-01' AND '2019-12-31';
```