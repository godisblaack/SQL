# 26 Self Outer Joins

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/5Xn-cBoT6RI?si=mwvfmvWt64sGeIJT"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Recap: Self Join with Inner Join

We use a **self join** to connect employees with their managers.

```sql
USE sql_hr;

SELECT 
    e.employee_id, 
    e.first_name, 
    m.first_name AS manager
FROM employees e
JOIN employees m 
    ON e.reports_to = m.employee_id;
```

### ✅ Result

* Shows each employee with their manager.
* ❌ Problem: Employees without managers (like the CEO) are excluded.

---

## 2. Why Are Some Employees Missing?

* The condition `e.reports_to = m.employee_id` only works if the employee **has a manager**.
* For the CEO (or top-level managers), `reports_to = NULL`.
* Since `NULL` does not match any `employee_id`, those employees are excluded.

---

## 3. Fix: Use LEFT JOIN

Switching to `LEFT JOIN` ensures that **all employees** are included, even those without managers.

```sql
SELECT 
    e.employee_id, 
    e.first_name, 
    m.first_name AS manager
FROM employees e
LEFT JOIN employees m 
    ON e.reports_to = m.employee_id;
```

### ✅ Result

* Every employee is listed.
* Employees without managers show `NULL` in the `manager` column.

---

## 4. Key Takeaways

* **Self join**: A table joins with itself.
* **Inner join**: Only employees with managers are shown.
* **Left join**: Shows all employees, even those without managers.
* Common real-world use:

  * Employee → Manager hierarchy
  * Organization charts

---

## Queries from the video

```sql
USE sql_hr;

SELECT 
    e.employee_id, e.first_name, m.first_name AS manager
FROM
    employees e
        JOIN
    employees m ON e.reports_to = m.employee_id;
    
SELECT 
    e.employee_id, e.first_name, m.first_name AS manager
FROM
    employees e
        LEFT JOIN
    employees m ON e.reports_to = m.employee_id;
```