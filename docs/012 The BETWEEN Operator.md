# 12 The BETWEEN Operator

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/_K-IB9UVkAs?si=tlunP0jj6skk-O8O"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

### **Using the `BETWEEN` Operator in SQL**

The `BETWEEN` operator in SQL is used to filter data within a specific range. It can be used with numerical, date, and even text values, making it a versatile tool for querying data. One key point to note is that the `BETWEEN` operator is **inclusive**, meaning it includes both the lower and upper bounds in the range.

---

### **Using `BETWEEN` for Numeric Ranges**

The `BETWEEN` operator is a cleaner and more concise way to write queries when comparing values within a range. Instead of using `AND` with comparison operators like `>=` and `<=`, you can use `BETWEEN` for the same result.

#### **Example: Filtering Customers Based on Points**

For example, if you want to filter customers with points greater than or equal to 1,000 and less than or equal to 3,000, you could write:

```sql
SELECT * 
FROM customers
WHERE points >= 1000 AND points <= 3000;
```

This is equivalent to:

```sql
SELECT * 
FROM customers
WHERE points BETWEEN 1000 AND 3000;
```

Both queries will return the same result, but using `BETWEEN` is cleaner and easier to read.

---

### **Using `BETWEEN` for Date Ranges**

The `BETWEEN` operator is **not limited to numbers**. It can also be used with **date values**, making it extremely useful when querying time-based data.

#### **Example: Filtering Customers Based on Birth Dates**

Suppose you want to find customers born between January 1st, 1990, and January 1st, 2000. You can use `BETWEEN` like this:

```sql
SELECT * 
FROM customers
WHERE birth_date BETWEEN '1990-01-01' AND '2000-01-01';
```

This query will return customers whose birth dates are between these two dates, **inclusive** of both the start and end date.

---

### **Recap of Key Points**

* **`BETWEEN` Operator**: Filters data within a specified range.

  * **Inclusive** of both the lower and upper bounds.
  * Works with numbers, dates, and text.
* **Alternative to Multiple Comparison Operators**:
  Instead of writing:

  ```sql
  WHERE points >= 1000 AND points <= 3000;
  ```

  You can simply write:

  ```sql
  WHERE points BETWEEN 1000 AND 3000;
  ```

---

### **SQL Queries from the Video**

1. **Get customers whose points are between 1,000 and 3,000**:

   ```sql
   USE sql_store;

   SELECT * 
   FROM customers
   WHERE points BETWEEN 1000 AND 3000;
   ```

   Equivalent to:

   ```sql
   SELECT * 
   FROM customers
   WHERE points >= 1000 AND points <= 3000;
   ```

2. **Exercise: Get customers born between January 1st, 1990, and January 1st, 2000**:

   ```sql
   SELECT * 
   FROM customers
   WHERE birth_date BETWEEN '1990-01-01' AND '2000-01-01';
   ```

---

### **Conclusion**

The `BETWEEN` operator makes filtering ranges much easier to read and write. Whether you're filtering numeric values, dates, or other ranges, it helps make your queries more concise and expressive.

---

## Queries from the video

```sql
USE sql_store;

SELECT 
    *
FROM
    customers
WHERE
    points BETWEEN 1000 AND 3000;
	-- points >= 1000 AND points <= 3000;
```
## Exercise

### Return customers born between 1/1/1990 and 1/1/2000

```sql
USE sql_store;

SELECT 
    *
FROM
    customers
WHERE
    birth_date BETWEEN '1990-01-01' AND '2000-01-01';
```