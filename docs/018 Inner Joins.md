# 18 Inner Joins

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/xpeREZ19xKU?si=Q_hY0D8fHjRXVYog"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## Why Do We Join Tables?

* In relational databases, data is often stored across **multiple tables** to reduce redundancy.
* Example:

  * `orders` table → contains order information and a `customer_id`.
  * `customers` table → contains customer details like `first_name`, `last_name`, `email`.
* To display **order + customer info** together, we need to **join** the two tables.

---

## INNER JOIN Basics

* `JOIN` in MySQL defaults to `INNER JOIN`.
* Combines rows from two tables where a condition is satisfied (usually equality of foreign key and primary key).

### Example: Join Orders with Customers

```sql
SELECT 
    *
FROM
    orders
    JOIN customers ON orders.customer_id = customers.customer_id;
```

✅ **Result:** Shows all columns from `orders`, then all columns from `customers`, matched by `customer_id`.

---

## Selecting Specific Columns

We usually don’t need all columns — instead, we pick the ones we care about.

```sql
SELECT 
    order_id, first_name, last_name
FROM
    orders
    JOIN customers ON orders.customer_id = customers.customer_id;
```

✅ **Result:** Each order is shown with the customer’s first and last name.

---

## Ambiguous Column Problem

* If two tables have a column with the **same name** (e.g., `customer_id`), SQL doesn’t know which one to use.
* Query example that causes an error:

```sql
SELECT 
    order_id, customer_id, first_name, last_name
FROM
    orders
    JOIN customers ON orders.customer_id = customers.customer_id;
```

❌ Error:
`Error Code: 1052. Column 'customer_id' in field list is ambiguous`

### Solution → Qualify Columns

Specify the table name (or alias):

```sql
SELECT 
    order_id, orders.customer_id, first_name, last_name
FROM
    orders
    JOIN customers ON orders.customer_id = customers.customer_id;
```

---

## Using Table Aliases

* To simplify queries, assign **short aliases** to table names.
* Convention: use the first letter(s) of the table name.

```sql
SELECT 
    order_id, o.customer_id, first_name, last_name
FROM
    orders o
    JOIN customers c ON o.customer_id = c.customer_id;
```

✅ Cleaner and easier to read.

---

## Exercise – Joining `order_items` and `products`

### Problem

For each order item:

* Show the `order_id`
* Show the `product_id` and `name`
* Show the **total price** (calculated as `quantity * unit_price` from `order_items`)

### Solution

```sql
SELECT 
    order_id, p.product_id, name, quantity * oi.unit_price AS total_price
FROM
    order_items oi
    JOIN products p ON oi.product_id = p.product_id;
```

---

## Why Two `unit_price` Columns?

* `products.unit_price` → current price of the product.
* `order_items.unit_price` → snapshot of price **at the time of order**.
* For accurate reporting, always use `order_items.unit_price` when calculating order totals.

---

## Key Takeaways

* Use `JOIN` (or `INNER JOIN`) to combine rows from multiple tables.
* Always use the `ON` condition to specify how tables relate (typically foreign key = primary key).
* If columns exist in both tables, qualify them with table names or aliases.
* Use **aliases** (`o`, `c`, `oi`, `p`) to make queries shorter and cleaner.
* For financial calculations, always use historical prices stored in order-related tables, not current product prices.

---

## Queries from the video

```sql
USE sql_store;

SELECT 
    *
FROM
    orders
        JOIN
    customers ON orders.customer_id = customers.customer_id;

-- Here JOIN means INNER JOIN

SELECT 
    order_id, first_name, last_name
FROM
    orders
        JOIN
    customers ON orders.customer_id = customers.customer_id;

/* 
Query below will give the following error:
Error Code: 1052. Column 'customer_id' in field list is ambiguous

We are getting this error because customer_id column is present in both orders and customers 
tables, and query doesn't know that from which table it should choose customer_id column.

SELECT 
    order_id, customer_id, first_name, last_name
FROM
    orders
        JOIN
    customers ON orders.customer_id = customers.customer_id;
*/

SELECT 
    order_id, orders.customer_id, first_name, last_name
FROM
    orders
        JOIN
    customers ON orders.customer_id = customers.customer_id;

-- The name of the tables have been repeated in several places. We can give them alias
-- as shown below:


SELECT 
    order_id, o.customer_id, first_name, last_name
FROM
    orders o
        JOIN
    customers c ON o.customer_id = c.customer_id;

```

## Exercise 

### JOIN `order_items` and `products table` and show the product_id with their names and total price

```sql
USE sql_store;

SELECT 
    order_id, p.product_id, name, quantity * oi.unit_price AS total_price
FROM
    order_items oi
    JOIN products p ON oi.product_id = p.product_id;
```