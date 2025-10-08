# 33 Inserting Multiple Rows

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/bsYjQGwUoag?si=Ert0kfrtjWA2T2Ze"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Why Insert Multiple Rows?

Instead of executing several separate `INSERT` statements, we can insert **multiple rows in one go**, which is:

* **Faster** → fewer round trips to the database.
* **Cleaner** → shorter, easier-to-read code.
* **Efficient** → especially useful for batch loading.

---

## 2. Example with `shippers` Table

**Table definition recap**:

* `shipper_id`: Primary Key, `INT`, Auto Increment.
* `name`: `VARCHAR`, required.

Since `shipper_id` is auto-incremented, we only need to insert into the `name` column.

```sql
USE sql_store;

INSERT INTO shippers (name)
VALUES ('Shipper1'),
       ('Shipper2'),
       ('Shipper3');
```

✅ MySQL automatically assigns IDs for each row (e.g., 6, 7, 8 if there were already 5 rows).

---

## 3. General Syntax

```sql
INSERT INTO table_name (col1, col2, col3)
VALUES (val1, val2, val3),
       (val1, val2, val3),
       (val1, val2, val3);
```

* Columns must be listed once.
* Each row’s values are inside `()` and separated by commas.

---

## 4. Exercise: Insert 3 products into `products` Table

**Products table columns**:

* `product_id` → Auto Increment (skip it).
* `name` → product name.
* `quantity_in_stock` → stock level.
* `unit_price` → price per unit.

```sql
INSERT INTO products (name, quantity_in_stock, unit_price)
VALUES ('Product1', 10, 1.95),
       ('Product2', 11, 1.95),
       ('Product3', 12, 1.95);
```

---

## 5. Important Note on Auto Increment IDs

* Even if you delete rows, MySQL does **not reuse old IDs**.
* It always increments from the **last used value**.
* Example: If the last row inserted was `product_id = 14` and you delete it, the next row inserted will get `15`, not `14`.

---

## 6. Queries from the video

```sql
USE sql_store;

INSERT INTO 
    shippers (name)
VALUES ('Shipper1'),
	   ('Shipper2'),
	   ('Shipper3');
```

---

## 7. Exercise

### Insert three rows in the products table

```sql
USE sql_stores;

INSERT INTO 
    products
VALUES (DEFAULT, 'Small Bread', 10, 1.49),
	   (DEFAULT, 'Medium Bread', 10, 1.99),
       (DEFAULT, 'Big Bread', 10, 2.99);
       
-- Solution by Mosh

INSERT INTO 
    products (
        name, 
        quantity_in_stock, 
        unit_price
    )
VALUES ('Product1', 10, 1.95),
	   ('Product2', 11, 1.95),
       ('Product3', 12, 1.95);
```