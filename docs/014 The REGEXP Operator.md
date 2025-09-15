# 14 The REGEXP Operator

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/7u2X91rqeH0?si=X_RV_-Jk54ul90Xh"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

### **Using `REGEXP` in SQL for Complex String Matching**

In SQL, the `REGEXP` (short for regular expression) operator allows us to search for complex patterns in strings, which goes beyond the functionality of the `LIKE` operator. While `LIKE` can match basic patterns (like wildcards for substrings), `REGEXP` allows us to define more complex search criteria using regular expressions.

### **Key Concepts in Regular Expressions**

* **`^`**: Represents the **start** of a string.
* **`$`**: Represents the **end** of a string.
* **`|` (Pipe)**: Represents the **OR** operator, allowing us to match multiple patterns.
* **`[]` (Square Brackets)**: Used to match any **single character** within a specified range or set.
* **Character ranges in square brackets**: For example, `[a-h]` matches any character between `a` and `h`.

### **Using Regular Expressions in SQL Queries**

#### **Examples**

1. **Searching for the word "field" anywhere in the last name**:
   If we want to match the word "field" anywhere in the last name, we can use the following:

   ```sql
   SELECT *
   FROM customers
   WHERE last_name REGEXP 'field';
   ```

2. **Customers whose last name ends with "field", "mac", or "rose"**:
   To find customers whose last name ends with "field", "mac", or "rose", we can use:

   ```sql
   SELECT *
   FROM customers
   WHERE last_name REGEXP 'field$|mac$|rose$';
   ```

   * `$` indicates that the match must occur at the **end** of the string.
   * The pipe (`|`) acts as a logical OR between the different patterns.

3. **Customers whose last name starts with "field", "mac", or "rose"**:
   If we want customers whose last name starts with "field", "mac", or "rose", we use:

   ```sql
   SELECT *
   FROM customers
   WHERE last_name REGEXP '^field|^mac|^rose';
   ```

   * `^` indicates that the match must occur at the **start** of the string.

4. **Customers whose last name contains "field" or "mac"**:
   If we want customers whose last name contains "field" or "mac" anywhere, we use:

   ```sql
   SELECT *
   FROM customers
   WHERE last_name REGEXP 'field|mac';
   ```

---

### **Regular Expressions Exercises from the Video**

Let's go over the exercises that were discussed in the video.

---

### **1. Customers whose first name is "ELKA" or "AMBUR"**

We can use the pipe (`|`) to represent the logical OR between "ELKA" and "AMBUR":

```sql
USE sql_store;

SELECT *
FROM customers
WHERE first_name REGEXP 'ELKA|AMBUR';
```

* This query matches customers whose first name is either **ELKA** or **AMBUR**.

---

### **2. Customers whose last name ends with "EY" or "ON"**

We use the dollar sign (`$`) to indicate the end of a string:

```sql
USE sql_store;

SELECT *
FROM customers
WHERE last_name REGEXP 'EY$|ON$';
```

* The `$` ensures that the match is at the **end** of the string.
* This query returns customers whose last name ends with either **EY** or **ON**.

---

### **3. Customers whose last name starts with "MY" or contains "SE"**

We use the caret (`^`) to represent the start of a string and simply match for "SE" anywhere in the name:

```sql
USE sql_store;

SELECT *
FROM customers
WHERE last_name REGEXP '^MY|SE';
```

* `^MY` matches last names that **start** with "MY".
* `SE` matches last names that **contain** "SE" anywhere.

---

### **4. Customers whose last name contains "B" followed by "R" or "U"**

We can use square brackets (`[]`) to specify that the character after "B" can either be "R" or "U":

```sql
USE sql_store;

SELECT *
FROM customers
WHERE last_name REGEXP 'B[R|U]';
-- or equivalently
SELECT *
FROM customers
WHERE last_name REGEXP 'BR|BU';
```

* This will match customers whose last name contains **B** followed by either **R** or **U**.

---

### **Additional Notes on Regular Expressions in MySQL**

* **Character Classes**: You can use a character class like `[a-h]` to match any letter between `a` and `h`.

  Example:

  ```sql
  SELECT *
  FROM customers
  WHERE last_name REGEXP '[a-h]e';
  ```

  * This matches last names that have any character between `a` and `h` followed by "e".

* **Grouping**: You can group parts of your regular expression using parentheses `()` to create subexpressions.

---

### **Conclusion**

Using `REGEXP` gives you much more flexibility than `LIKE` for matching complex patterns in strings. It allows for things like logical ORs (`|`), character classes (`[]`), and more powerful pattern matching with `^` and `$` for the beginning and end of strings, respectively. Once you get familiar with regular expressions, you'll find them indispensable for more advanced string searching in SQL.

---

## Queries

```sql
USE sql_store;

SELECT 
    *
FROM
    customers
WHERE
	last_name REGEXP '[a-h]e';
	-- last_name REGEXP 'e[fmq]';
	-- last_name REGEXP '[gim]e';
	-- last_name REGEXP 'field$|mac|rose';
	-- last_name REGEXP '^field|mac|rose';
	-- last_name REGEXP 'field|mac|rose';
    -- last_name REGEXP 'field|mac';
    -- last_name REGEXP 'field' OR 'mac';
    -- last_name REGEXP 'field$';
    -- last_name REGEXP '^field';
	-- last_name LIKE '%field%';
```

## Exercise

### 1. Return All the Records Where First Names are ELKA or AMBUR
```sql
USE sql_store;

SELECT 
    *
FROM
    customers
WHERE
    first_name REGEXP 'ELKA|AMBUR';
```
- Matches customers whose `first_name` is **ELKA** or **AMBUR**.

---

### 2. Return All the Records With Last Names Ending with EY or ON
```sql
USE sql_store;

SELECT 
    *
FROM
    customers
WHERE
    last_name REGEXP 'EY$|ON$';
```
- `$` ensures the match is at the **end** of the string.
- Matches last names ending in **EY** or **ON**.

---

### 3. Return All the Records With Last Names Starting with MY or Containing SE
```sql
USE sql_store;

SELECT 
    *
FROM
    customers
WHERE
    last_name REGEXP '^MY|SE';
```
- `^MY` → Starts with "MY".
- `SE` → Contains "SE" anywhere in the string.

---

### 4. Return All the Records With Last Names Containing B Followed by R or U
```sql
USE sql_store;

SELECT 
    *
FROM
    customers
WHERE
    last_name REGEXP 'B[R|U]';
    -- last_name REGEXP 'BR|BU';
```
- `[R|U]` → Matches **R** or **U** after **B**.
- Equivalent to `'BR|BU'`.

---