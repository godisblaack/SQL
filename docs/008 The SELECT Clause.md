# 8 The SELECT Clause

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/JzwV6x_BML4?si=fm0FeBTsKgIkuug0"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. **Selecting Specific Columns**:

   * Instead of retrieving all columns with `*`, you can specify the exact columns you need to avoid unnecessary data retrieval, especially for large datasets.

   ```sql
   SELECT first_name, last_name
   FROM customers;
   ```

## 2. **Using Arithmetic Expressions**:

   * SQL allows you to perform calculations directly in the `SELECT` clause.
   * Example: You can add, multiply, subtract, divide, or use modulo to perform arithmetic on column values.
   * For example, to calculate a discount factor based on the points:

    ```sql
    SELECT 
        last_name, 
        first_name, 
        points, 
        (points + 10) * 100 AS discount_factor
    FROM 
        customers;
    ```

   * You can use single quotes or double quotes to have space in the column name, as shown below:

    ```sql
    SELECT 
        last_name, 
        first_name, 
        points, 
        (points + 10) * 100 AS 'discount factor'
    FROM 
        customers;
    ```

    ```sql
    SELECT 
        last_name, 
        first_name, 
        points, 
        (points + 10) * 100 AS "discount factor"
    FROM 
        customers;
    ```

   * In this case, the **points** column is manipulated with an arithmetic expression: `(points + 10) * 100`.

## 3. **Changing the Order of Operations with Parentheses**:

   * The order of operators in arithmetic expressions follows standard mathematical precedence. If needed, you can use parentheses to control the order.
   * For instance:

   ```sql
   SELECT 
       points * 10 + 100 AS 'new_points'
   FROM 
       customers;
   ```

   * You can also use parentheses to re-order operations for clarity:

   ```sql
   SELECT 
       (points + 10) * 100 AS 'discount factor'
   FROM 
       customers;
   ```

## 4. **Using Aliases to Rename Columns**:

   * To give your calculated columns or existing columns more meaningful names, you can use **column aliases** with the `AS` keyword.
   * For example, naming the calculated discount column `discount_factor`:

   ```sql
   SELECT 
       last_name, 
       first_name, 
       points, 
       (points + 10) * 100 AS 'discount factor'
   FROM 
       customers;
   ```

   * If you want spaces in column aliases, you can enclose the alias in single or double quotes:

   ```sql
   SELECT 
       (points + 10) * 100 AS 'discount factor'
   FROM 
       customers;
   ```

## 5. **Using `DISTINCT` to Remove Duplicates**:

   * If you want to retrieve unique values from a column (removing duplicates), use the `DISTINCT` keyword.
   * Example:

   ```sql
   SELECT DISTINCT state
   FROM customers;
   ```

   * This would return only the unique states from the **customers** table.

---

### **SQL Queries from the Tutorial**

Here’s the SQL code presented in the tutorial:

1. **Basic Query with Arithmetic Expression**:

   * The `SELECT` statement that retrieves `last_name`, `first_name`, `points`, and a calculated column called `discount_factor`:

   ```sql
   USE sql_store;

   SELECT 
       last_name,
       first_name,
       points,
       (points + 10) * 100 AS 'discount factor'
   FROM
       customers;
   ```

2. **Using `DISTINCT` to Retrieve Unique States**:

   * This query retrieves the unique states from the **customers** table:

   ```sql
    SELECT DISTINCT
        state
    FROM
        customers
   ```

---

### **Exercise Solution**

The exercise asks you to write a query that:

* Retrieves all **products**.
* Shows the **name**, **unit price**, and a new calculated column called **new_price**, which is the unit price increased by 10%.

Here’s the solution:

```sql
SELECT 
    name, 
    unit_price, 
    unit_price * 1.1 AS new_price
FROM 
    products;
```

* **Explanation**:

  * We are selecting `name` and `unit_price` from the **products** table.
  * We calculate the `new_price` by multiplying `unit_price` by `1.1` (representing a 10% increase) and aliasing this as `new_price`.

---

### **Recap: Key Points**

* **Explicitly selecting columns** is better than using `*`, especially for large tables.
* **Arithmetic expressions** can be used for on-the-fly calculations.
* **Parentheses** can control the order of operations in calculations.
* **Aliases** are helpful for giving descriptive names to columns, especially when using calculations.
* Use **`DISTINCT`** to remove duplicates in your query results.

---

## Queries from the video
```sql
USE sql_store;
```
```sql
SELECT 
    last_name, 
    first_name, 
    points, 
    (points + 10) * 100 AS discount_factor
FROM 
    customers;
```
```sql
SELECT 
    last_name,
    first_name,
    points,
    (points + 10) * 100 AS 'discount factor'
FROM
    customers;
```
```sql
SELECT 
    state
FROM
    customers
```
```sql
SELECT DISTINCT
    state
FROM
    customers
```

## Exercise

### **Task:** Return all the products with `name`, `unit_price`, and `new price` where  
`new price = unit_price * 1.1`.

```sql
USE sql_store;

SELECT 
    name, 
    unit_price, 
    unit_price * 1.1 AS new_price
FROM 
    products;
```