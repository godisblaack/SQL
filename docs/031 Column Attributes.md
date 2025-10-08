# 31 Column Attributes

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/1UokoqEt4bs?si=GQMJHjsk82_QkEGE"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Looking at Table Design

When opening a table (e.g., `customers`) in **design mode**, you see several attributes for each column:

### a) Column Name

* Defines what the column stores (e.g., `customer_id`, `first_name`).

### b) Data Type

* Specifies the **kind of values** allowed in that column.
* Common examples:

  * `INT` → whole numbers (1, 2, 3 … no decimals).
  * `VARCHAR(n)` → variable-length text up to *n* characters.
  * `CHAR(n)` → fixed-length text (always uses *n* spaces, wastes storage if shorter).

👉 **Best practice** → use `VARCHAR` for most text columns to avoid wasted space.

### c) Primary Key (PK)

* A **unique identifier** for each row.
* In `customers`, `customer_id` is the **Primary Key**.
* Shown with a **yellow key icon**.

### d) Not Null (NN)

* Means the column **cannot be empty (NULL)**.
* Example: `customer_id`, `first_name`, and `last_name` must have values.
* Optional columns (like `birth_date`, `phone`) can accept NULL.

### e) Auto Increment (AI)

* Often used with **Primary Key** columns.
* Automatically generates a new value when inserting rows.
* Example: If last `customer_id` = 10, the next new row gets `11`.

### f) Default Value

* Value automatically assigned if none is provided.
* Example:

  * `birth_date` → default `NULL`.
  * `points` → default `0`.

---

## 2. Example: Customers Table Overview

| Column      | Data Type   | Key | NN | AI | Default |
| ----------- | ----------- | --- | -- | -- | ------- |
| customer_id | INT         | PK  | ✓  | ✓  | Auto    |
| first_name  | VARCHAR(50) |     | ✓  |    | –       |
| last_name   | VARCHAR(50) |     | ✓  |    | –       |
| birth_date  | DATE        |     |    |    | NULL    |
| phone       | VARCHAR(20) |     |    |    | NULL    |
| points      | INT         |     | ✓  |    | 0       |

---

## 3. Key Concepts Recap

* **Primary Key** → uniquely identifies each row.
* **Not Null** → prevents missing values.
* **Auto Increment** → database automatically assigns numeric IDs.
* **Default Values** → used when no value is provided.
* **VARCHAR vs CHAR** →

  * `VARCHAR(50)` → efficient storage, only uses required space.
  * `CHAR(50)` → always uses 50 characters, wastes space if shorter.