# 10 The AND, OR and NOT Operators

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/DLzOfLoDxSs?si=ExYYo6DW96vqS1Ym"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

### **Logical Operators in SQL**

1. **`AND` Operator**:

   * The `AND` operator is used when **both** conditions must be true for a record to be included in the result set.
   * **Example**: Get customers born after 1990 **and** with more than 1,000 points:

     ```sql
     SELECT * 
     FROM customers
     WHERE birth_date > '1990-01-01' AND points > 1000;
     ```

2. **`OR` Operator**:

   * The `OR` operator is used when **either** of the conditions must be true.
   * **Example**: Get customers born after 1990 **or** with more than 1,000 points:

     ```sql
     SELECT * 
     FROM customers
     WHERE birth_date > '1990-01-01' OR points > 1000;
     ```

3. **`NOT` Operator**:

   * The `NOT` operator negates a condition. If the condition is true, `NOT` makes it false, and vice versa.
   * **Example**: Get customers who **were not** born after 1990 and have less than 1,000 points:

     ```sql
     SELECT * 
     FROM customers
     WHERE NOT (birth_date > '1990-01-01' OR points > 1000);
     ```

     The negation of this would be:

     ```sql
     SELECT * 
     FROM customers
     WHERE birth_date <= '1990-01-01' AND points <= 1000;
     ```

4. **Order of Evaluation (Precedence)**:

   * **`NOT`** is evaluated first, then **`AND`**, and finally **`OR`**.
   * To control the order of evaluation, you can use **parentheses** to group conditions.
   * **Example**: If you want to ensure that the `AND` condition is evaluated before `OR`, use parentheses:

     ```sql
     SELECT * 
     FROM customers
     WHERE birth_date > '1990-01-01' OR (points > 1000 AND state = 'VA');
     ```

---

### **SQL Queries from the Tutorial**

1. **Customers Born After 1990 and With More Than 1,000 Points**:

   * This query retrieves customers who meet **both** conditions:

   ```sql
   USE sql_store;

   SELECT * 
   FROM customers
   WHERE birth_date > '1990-01-01' AND points > 1000;
   ```

2. **Customers Born After 1990 or With More Than 1,000 Points**:

   * This query retrieves customers who meet **either** condition:

   ```sql
   USE sql_store;

   SELECT * 
   FROM customers
   WHERE birth_date > '1990-01-01' OR points > 1000;
   ```

3. **Negating Conditions with `NOT`**:

   * This query negates the conditions to get customers **born before 1990** and with **less than 1,000 points**:

   ```sql
   USE sql_store;

   SELECT * 
   FROM customers
   WHERE NOT (birth_date > '1990-01-01' OR points > 1000);
   ```

   Which is equivalent to:

   ```sql
   USE sql_store;

   SELECT * 
   FROM customers
   WHERE birth_date <= '1990-01-01' AND points <= 1000;
   ```

4. **Customers Born After 1990 and With More Than 1,000 Points, or Living in Virginia**:

   * Using parentheses to control the logical operator precedence:

   ```sql
   USE sql_store;

   SELECT * 
   FROM customers
   WHERE birth_date > '1990-01-01' OR (points > 1000 AND state = 'VA');
   ```

---

### **Exercise: Query for Order Items With Total Price Greater Than 30**

**Problem**: You need to get the items from **order number 6** where the total price of the item is greater than **30**. This means multiplying the `quantity` by the `unit_price` and comparing the result.

Solution:

```sql
USE sql_store;

SELECT * 
FROM order_items
WHERE order_id = 6 AND unit_price * quantity > 30;
```

Explanation:

* `order_id = 6`: Filters only the items for **order number 6**.
* `unit_price * quantity > 30`: Filters only the items where the **total price** (unit price multiplied by quantity) is greater than 30.

---

### **Recap of Key Concepts**

* **Logical Operators**:

  * **`AND`**: Both conditions must be true.
  * **`OR`**: Either condition can be true.
  * **`NOT`**: Negates a condition.
* **Operator Precedence**:

  * **`NOT`** has the highest precedence, followed by **`AND`**, and then **`OR`**.
  * Use parentheses to change the order of evaluation and make your SQL queries more readable.

---

## Queries from the video

```sql
USE sql_store;

SELECT 
    *
FROM
    customers
WHERE
    birth_date <= '1990-01-01'
        AND points <= 1000;
	-- NOT (birth_date > '1990-01-01' OR points > 1000);
	-- birth_date > '1990-01-01' OR (points > 1000 AND state = 'VA');
    -- birth_date > '1990-01-01' AND points > 1000;
    -- birth_date > '1990-01-01' AND points > 1000;
```

## Exercise

### From the order_items table, get the items for order number 6 where the total price is greater than 30

```sql
USE sql_store;

SELECT 
    *
FROM
    order_items
WHERE
    order_id = 6 AND unit_price * quantity > 30;
```