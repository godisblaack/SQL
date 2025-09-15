# 9 The WHERE Clause

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/ZZsIPQ7R3cQ?si=cjccm9YejG8bDAdo"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. **Basic `WHERE` Clause Usage**:

   * The `WHERE` clause is used to filter records based on a specified condition.
   * For example, if you only want to select customers with points greater than 3,000:

   ```sql
   SELECT * 
   FROM customers
   WHERE points > 3000;
   ```

## 2. **Comparison Operators**:
   SQL provides several comparison operators to compare values in the `WHERE` clause:

   * **Greater than (`>`)** and **Greater than or equal to (`>=`)**
   * **Less than (`<`)** and **Less than or equal to (`<=`)**
   * **Equality (`=`)** and **Not equal (`<>` or `!=`)**

   Example:

   ```sql
   SELECT * 
   FROM customers
   WHERE state = 'Virginia';  -- Filters customers in Virginia
   ```

## 3. **String Comparison**:

   * When dealing with textual data, you must enclose values in **single or double quotes**.
   * Example: `WHERE state = 'Virginia'` (note the use of single quotes for the string).
   * SQL is **case-insensitive** by default, meaning `VA` and `va` are treated the same:

   ```sql
   WHERE state = 'va';  -- This would return the same result as 'VA'
   ```

## 4. **Date Comparison**:

   * Dates in SQL should also be enclosed in quotes, even though they are not technically strings.
   * The standard format for dates in MySQL is `YYYY-MM-DD`.
   * Example:

   ```sql
   SELECT * 
   FROM customers
   WHERE birth_date > '1990-01-01';  -- Filters customers born after January 1st, 1990
   ```

## 5. **Using the `NOT EQUAL` Operator**:

   * To filter out records that do **not** meet a specific condition, use the `<>` (or `!=`) operator.
   * Example:

   ```sql
   SELECT * 
   FROM customers
   WHERE state <> 'Virginia';  -- Customers who are NOT in Virginia
   ```

## 6. **Combining Multiple Conditions**:

   * In later tutorials, you will learn how to combine multiple conditions using `AND`, `OR`, and parentheses to create more complex filters.

---

### **SQL Queries from the Tutorial**

1. **Filtering Customers by Points**:
   This query retrieves customers with **points greater than 3,000**.

   ```sql
   USE sql_store;

   SELECT 
       *
   FROM
       customers
   WHERE
       points > 3000;
   ```

2. **Filtering Customers in a Specific State**:
   This example shows how to filter customers located in **Virginia**:

   ```sql
   SELECT 
       *
   FROM
       customers
   WHERE
       state = 'Virginia';  -- Filters for Virginia
   ```

3. **Filtering Customers Not in Virginia**:
   To get customers **outside of Virginia**, we use the **`<>` (not equal)** operator:

   ```sql
   SELECT 
       *
   FROM
       customers
   WHERE
       state <> 'Virginia';  -- Filters out Virginia
   ```

4. **Filtering Customers Born After a Specific Date**:
   Here we filter customers born after **January 1st, 1990**:

   ```sql
   SELECT 
       *
   FROM
       customers
   WHERE
       birth_date > '1990-01-01';  -- Filters by birth date
   ```

---

### **Exercise Solution: Filtering Orders by Date**

**Problem**: You need to get orders placed in the current year (assuming the year is **2019**). The `order_date` column in the **orders** table will be used for this.

Solution:

```sql
USE sql_store;

SELECT 
    *
FROM
    orders
WHERE
    order_date >= '2019-01-01';  -- Filters orders placed in 2019 or later
```

* **Explanation**:

  * The `WHERE` clause filters records based on the `order_date` being greater than or equal to **January 1st, 2019**.

> **Note**: The query is assuming the current year is **2019**, but when the year changes, this will need to be updated unless you use dynamic methods (which will be covered later).

---

### **Recap: Key Points**

* **Comparison Operators**: Use operators like `=`, `>`, `<`, `>=`, `<=`, `<>` to filter data based on conditions.
* **String Comparison**: String values need to be enclosed in **single quotes**.
* **Date Comparison**: Dates should also be enclosed in quotes and follow the format `YYYY-MM-DD`.
* **Case Sensitivity**: SQL is generally case-insensitive for string comparisons.

---

## Queries from the video

```sql
USE sql_store;

SELECT 
    *
FROM
    customers
WHERE
    points > 3000; 
    -- state = 'VA'; 
    -- state = 'va';
    -- state <> 'va';
    -- birth_date > '1990-01-01';
```

## Exercise

### Get the orders placed this year

```sql
USE sql_store;

SELECT 
    *
FROM
    orders
WHERE
    order_date >= '2019-01-01';
```