# 11 The IN Operator

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/bBY8LBCONBU?si=RAMaBaq9dfe3JFOd"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

### **The `IN` Operator**

The `IN` operator helps you to **match** a column value against a list of potential values in a single condition. This is much cleaner and more efficient than using multiple `OR` conditions.

#### **Without `IN`**:

You can achieve the same result by writing multiple `OR` conditions, but it can get long and harder to read.

Example:

```sql
SELECT * 
FROM customers
WHERE state = 'VA' OR state = 'FL' OR state = 'GA';
```

#### **With `IN`**:

Instead of writing the multiple `OR` conditions, you can use the `IN` operator, which allows you to specify a list of values.

Example:

```sql
SELECT * 
FROM customers
WHERE state IN ('VA', 'FL', 'GA');
```

This query will return customers from Virginia, Florida, and Georgia, which is much more concise.

### **Negating the `IN` Operator**

If you want to **exclude** values from a list, you can use `NOT IN`.

Example:

```sql
SELECT * 
FROM customers
WHERE state NOT IN ('VA', 'FL', 'GA');
```

This will return all customers **not** in Virginia, Florida, or Georgia.

### **Use Cases for `IN`**

The `IN` operator is useful when:

* You need to filter a column based on multiple possible values.
* You want your query to be more readable and avoid repetitive `OR` conditions.

---

### **SQL Queries from the Video**

1. **Customers Located in States Other Than Virginia, Florida, or Georgia**:

   * Using `NOT IN`:

   ```sql
   USE sql_store;

   SELECT * 
   FROM customers
   WHERE state NOT IN ('VA', 'FL', 'GA');
   ```

   * Equivalent to:

   ```sql
   SELECT * 
   FROM customers
   WHERE state = 'VA' OR state = 'GA' OR state = 'FL';
   ```

2. **Customers Located in Virginia, Florida, or Georgia**:

   * Using `IN`:

   ```sql
   USE sql_store;

   SELECT * 
   FROM customers
   WHERE state IN ('VA', 'FL', 'GA');
   ```

---

### **Exercise: Using the `IN` Operator with Products**

**Problem**: Get the products where the `quantity_in_stock` is one of the following values: 49, 38, or 72.

Solution:

```sql
USE sql_store;

SELECT * 
FROM products
WHERE quantity_in_stock IN (49, 38, 72);
```

Explanation:

* **`quantity_in_stock IN (49, 38, 72)`**: This will return products whose stock quantity is either 49, 38, or 72.

---

### **Recap**

* The `IN` operator allows you to match a column's value against a list of values.
* It's a more efficient and readable way to handle multiple `OR` conditions.
* You can use `NOT IN` to exclude values from the result set.
* For large datasets or long queries, `IN` is great for improving both the performance and readability of your SQL.

---

## Queries from the video

```sql
USE sql_store;

SELECT 
    *
FROM
    customers
WHERE
	state NOT IN ('VA', 'FL', 'GA');
	-- state IN ('VA', 'FL', 'GA');
    -- state = 'VA' OR state = 'GA' OR state = 'FL';
```

## Exercise

### Return products with quantity in stock equal to 49, 38, 72

```sql
USE sql_store;

SELECT 
    *
FROM
    products
WHERE
	quantity_in_stock IN (49, 38, 72);
```