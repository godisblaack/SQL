# 20 Self Joins

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/aJHu6RVLwf0?si=Ner2qrCcCtFkm-Ka"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## Real-World Context

* Sometimes, a table needs to reference itself.
* Example: **Employee-Manager relationships**.
* In the `sql_hr` database, we have an `employees` table with the following relevant columns:

  * `employee_id` → unique ID for each employee.
  * `first_name`, `last_name`, `job_title`, `salary`.
  * `reports_to` → stores the `employee_id` of the **manager** for this employee.

---

## Why Use a Self Join?

* Avoids duplicating manager details (phone, address, salary, etc.) in the same row.
* Keeps **data normalized**:

  * Each employee has one record.
  * The manager is also an employee (so their details are in the same table).
* Allows us to query **employee–manager hierarchies** easily.

---

## Example: CEO vs Employees

* If `reports_to` is `NULL` → that employee has no manager (e.g., **CEO**).
* Otherwise, `reports_to` points to another employee’s `employee_id`.

---

## Query 1 – Basic Self Join

```sql
USE sql_hr;

SELECT 
    *
FROM
    employees e
    JOIN employees m 
        ON e.reports_to = m.employee_id;
```

✅ Explanation:

* We alias the first instance of the table as `e` (employees).
* We alias the second instance as `m` (managers).
* Join condition: `e.reports_to = m.employee_id`.
* Result: shows all employees **along with their managers** (all columns duplicated).

---

## Query 2 – Simplified Columns

```sql
SELECT 
    e.employee_id, 
    e.first_name, 
    m.first_name
FROM
    employees e
    JOIN employees m 
        ON e.reports_to = m.employee_id;
```

✅ Explanation:

* Select only a few columns: employee ID, employee name, and manager’s name.
* Still requires **prefixing with aliases** because the same column names exist in both tables.

---

## Query 3 – Using Column Aliases for Clarity

```sql
SELECT 
    e.employee_id, 
    e.first_name, 
    m.first_name AS manager
FROM
    employees e
    JOIN employees m 
        ON e.reports_to = m.employee_id;
```

✅ Explanation:

* Adds `AS manager` to rename the manager’s first name column.
* Final output is more readable:

  * `employee_id`
  * `first_name` (employee)
  * `manager` (manager’s first name)

---

## Key Principles of Self Joins

* **Aliases are mandatory** because the same table is referenced twice.
* Every column should be qualified with its alias (`e.` or `m.`).
* Self joins are used for:

  * **Hierarchies** (employee–manager, category–subcategory).
  * **Recursive relationships** (e.g., org charts, bill of materials).

---

## Example Output (simplified)

| employee\_id | first\_name | manager    |
| ------------ | ----------- | ---------- |
| 37270        | John        | NULL (CEO) |
| 37271        | Alice       | John       |
| 37272        | Bob         | John       |
| 37273        | Carol       | Alice      |

---

## Key Takeaways

* A **self join** = joining a table with itself.
* Essential for hierarchical or recursive data models.
* Use **aliases** to differentiate between roles (e.g., `employee` vs `manager`).

---

## Queries from the video

```sql
USE sql_hr;

SELECT 
    *
FROM
    employees e
        JOIN
    employees m ON e.reports_to = m.employee_id;
    
SELECT 
    e.employee_id, e.first_name, m.first_name
FROM
    employees e
        JOIN
    employees m ON e.reports_to = m.employee_id;

SELECT 
    e.employee_id, e.first_name, m.first_name AS manager
FROM
    employees e
        JOIN
    employees m ON e.reports_to = m.employee_id;
```