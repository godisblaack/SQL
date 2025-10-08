# 32 Inserting a Row

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/D7zejVYX_3A?si=KaFt1WIqBcDobdvq"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Reviewing the `customers` Table Structure

Before inserting data, it’s important to understand the column properties:

* **`customer_id`** → `INT`, `PK`, `NN`, `AI`

  * Primary Key, cannot be `NULL`, Auto Increments automatically.
* **`first_name`, `last_name`** → `VARCHAR(50)`, `NN`

  * Required string fields.
* **`birth_date`** → `DATE`, nullable, default = `NULL`.
* **`phone`** → `VARCHAR`, nullable, default = `NULL`.
* **`points`** → `INT`, not null, default = `0`.
* **Other columns**: `address`, `city`, `state` (all required).

---

## 2. Basic Insert Statement

The general syntax:

```sql
INSERT INTO table_name
VALUES (value1, value2, ..., valueN);
```

⚠️ **Problem**: You must supply values for **all columns in the correct order**, which can be error-prone.

---

## 3. Example: Insert with All Values

```sql
USE sql_store;

INSERT INTO customers
VALUES (
    DEFAULT,          -- customer_id (AI)
    'John',           -- first_name
    'Smith',          -- last_name
    '1990-01-01',     -- birth_date
    NULL,             -- phone
    'address',        -- address
    'city',           -- city
    'CA',             -- state
    DEFAULT           -- points
);
```

### Key Points

* `DEFAULT` → tells MySQL to auto-generate the value (`customer_id`, `points`).
* `NULL` → explicitly insert a `NULL` into nullable columns.
* Strings must be wrapped in quotes (`'John'`, `'CA'`).
* Date format → `'YYYY-MM-DD'`.

---

## 4. Insert by Specifying Columns

To avoid supplying values for all columns, you can explicitly list the columns you want to insert into:

```sql
INSERT INTO customers (
    first_name,
    last_name,
    birth_date,
    address,
    city,
    state
) VALUES (
    'John',
    'Smith',
    '1990-01-01',
    'address',
    'city',
    'CA'
);
```

### Why This is Better?

* You don’t need to provide values for every column.
* More readable and flexible.
* Safer if the table structure changes in the future.

---

## 5. Changing Column Order

The order in the `INSERT` statement doesn’t have to match the table definition, **as long as you match the order of columns in your query with the values provided**:

```sql
INSERT INTO customers (
    last_name,
    first_name,
    birth_date,
    address,
    city,
    state
) VALUES (
    'Smith',
    'John',
    '1990-01-01',
    'address',
    'city',
    'CA'
);
```

👉 Here we flipped `last_name` and `first_name`. The values are correctly aligned because we explicitly listed the column order.

---

## 6. Best Practices for Insert

* Always **specify column names** instead of using plain `VALUES (…)`.
* Use `DEFAULT` for auto-incremented IDs or fields with default values.
* Use `NULL` only if the column allows it.
* Ensure string and date formats are correct.

---

## 7. Queries form the video

```sql
USE sql_store;

INSERT INTO customers
VALUES (
	DEFAULT,
	'John',
    'Smith',
    '1990-01-01', -- We can also use NULL keyword
    NULL, -- We can also use DEFAULT keyword
    'address',
    'city',
    'CA',
    DEFAULT
    );

-- Defining specific columns and its data

INSERT INTO customers (
	first_name,
    last_name,
    birth_date,
    address,
    city,
    state)
VALUES (
	'John',
    'Smith',
    '1990-01-01',
    'address',
    'city',
    'CA'
    );
    
-- Different column order

INSERT INTO customers (
	last_name,
    first_name,
    birth_date,
    address,
    city,
    state)
VALUES (
	'Smith',
    'John',
    '1990-01-01',
    'address',
    'city',
    'CA'
    );
```