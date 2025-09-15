# 13 The LIKE Operator

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/boNvm4KyRNY?si=AJj3Hv_N-xhLNZz8"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

### **Using the `LIKE` Operator in SQL**

The `LIKE` operator in SQL allows us to filter rows based on string patterns. It's a great tool for searching when you don't know the exact value but want to match a part of the string. The `LIKE` operator uses **wildcards** to match these patterns:

* **Percent sign (`%`)**: Represents **zero or more characters**.
* **Underscore (`_`)**: Represents **exactly one character**.

### **Using the `LIKE` Operator with Wildcards**

#### **Examples**

1. **Customers whose last name starts with "B"**:
   To match customers whose last name starts with the letter "B" (case-insensitive), you can use:

   ```sql
   SELECT *
   FROM customers
   WHERE last_name LIKE 'B%';
   ```

   This will match any last name starting with "B" followed by zero or more characters.

2. **Customers whose last name contains "B"**:
   If you want to find customers whose last name contains the letter "B" anywhere, use:

   ```sql
   SELECT *
   FROM customers
   WHERE last_name LIKE '%B%';
   ```

   The percent signs (`%`) indicate that there can be any number of characters before and after "B".

3. **Customers whose last name ends with "Y"**:
   To find customers whose last name ends with "Y", you would use:

   ```sql
   SELECT *
   FROM customers
   WHERE last_name LIKE '%y';
   ```

4. **Customers whose last name is exactly 2 characters long and ends with "Y"**:
   If you want to find customers whose last name has exactly 2 characters and the second character is "Y", you can use the underscore (`_`) wildcard:

   ```sql
   SELECT *
   FROM customers
   WHERE last_name LIKE '_y';
   ```

5. **Customers whose last name has exactly 6 characters, starts with any character, and ends with "Y"**:
   For last names exactly 6 characters long, where the last character is "Y", you can use five underscores (`_`):

   ```sql
   SELECT *
   FROM customers
   WHERE last_name LIKE '_____y';
   ```

6. **Customers whose last name starts with "B" and is followed by exactly 4 characters before ending with "Y"**:
   This query uses both the underscore and percent sign:

   ```sql
   SELECT *
   FROM customers
   WHERE last_name LIKE 'b____y';
   ```

---

### **Practical Examples from the Video**

1. **Exercise 1: Get customers whose addresses contain "Trail" or "Avenue"**:
   To find customers whose address contains either "Trail" or "Avenue", we can use:

   ```sql
   SELECT *
   FROM customers
   WHERE address LIKE '%trail%' 
      OR address LIKE '%avenue%';
   ```

   This will return all customers with "trail" or "avenue" anywhere in their address.

2. **Exercise 2: Get customers whose phone numbers end with 9**:
   To filter customers whose phone numbers end with the digit "9", you can use:

   ```sql
   SELECT *
   FROM customers
   WHERE phone LIKE '%9';
   ```

   The percent sign (`%`) means that the phone number can have any number of digits before the last "9".

---

### **Using `NOT LIKE` for Excluding Patterns**

You can also use the `NOT LIKE` operator if you want to exclude certain patterns. For example, to find customers whose phone numbers **don't** end with "9":

```sql
SELECT *
FROM customers
WHERE phone NOT LIKE '%9';
```

This will return all customers whose phone numbers do not end with the digit "9".

---

### **Key Points to Remember**

* **Percent (`%`)**: Matches **zero or more characters**.
* **Underscore (`_`)**: Matches **exactly one character**.
* **Case Insensitivity**: In MySQL, the `LIKE` operator is case-insensitive by default (in some databases, you may need to use `ILIKE` for case-insensitive searches).
* **`NOT LIKE`**: Used to exclude rows that match the specified pattern.

---

### **SQL Queries from the Video**

#### **1. Get customers whose last name starts with "B" and ends with "Y"**:

```sql
USE sql_store;

SELECT *
FROM customers
WHERE last_name LIKE 'b____y';
```

#### **2. Exercise 1: Get customers whose addresses contain "Trail" or "Avenue"**:

```sql
SELECT *
FROM customers
WHERE address LIKE '%trail%'
   OR address LIKE '%avenue%';
```

#### **3. Exercise 2: Get customers whose phone numbers end with "9"**:

```sql
USE sql_store;

SELECT *
FROM customers
WHERE phone LIKE '%9';
```

---

### **Conclusion**

The `LIKE` operator is a powerful tool for pattern matching in SQL. Whether you're looking for substrings, specific patterns, or exclusions, it offers a flexible way to filter results. Try practicing with different wildcards and see how they help you refine your queries.

---

## Queries from the video

```sql
USE sql_store;

SELECT 
    *
FROM
    customers
WHERE
	last_name LIKE 'b____y';
	-- last_name LIKE '_____y';
	-- last_name LIKE '_y';
	-- last_name LIKE '%y';
	-- last_name LIKE '%b%';
 	-- last_name LIKE 'brush%';
    -- last_name LIKE 'b%';
```

## Exercise

### 1. Get the customers whose addresses constain TRAIL or AVENUE

```sql
SELECT 
    *
FROM
    customers
WHERE
    address LIKE '%trail%'
        OR address LIKE '%avenue%';
```

### 2. Get the customers whose phone numbers end with 9

```sql
USE sql_store;

SELECT 
    *
FROM
    customers
WHERE
    phone LIKE '%9';
```