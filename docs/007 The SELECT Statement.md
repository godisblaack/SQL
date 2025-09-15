# 7 The SELECT Statement

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/TTDUCITpZVA?si=VpBrwCbEOjjcX7ZG"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## Retrieving Data from a Single Table

### **Step-by-Step Breakdown**

1. **Selecting the Database**:

   * The first thing you need to do before running any queries is to **select the database**. In MySQL Workbench, this can be done either by:

     * **Typing a `USE` statement** to select a specific database:

       ```sql
       USE sql_store;
       ```
     * **Double-clicking on the database** in the Navigator panel.
   * This ensures that the queries you write will be executed against the selected database.

2. **Basic SELECT Query Structure**:

   * A basic **SELECT** query is used to retrieve data from a table.
   * The basic structure of the query is as follows:

     ```sql
     SELECT column1, column2, ... 
     FROM table_name;
     ```
   * You can use the `*` (asterisk) to select all columns in a table:

     ```sql
     SELECT * 
     FROM customers;
     ```

3. **Executing the Query**:

   * After writing the query, you can execute it by clicking the **Execute** button in MySQL Workbench, or by using the keyboard shortcut:

     * **Mac**: `Shift + Command + Enter`
     * **Windows**: (The shortcut is different on Windows, usually `Ctrl + Enter` or `Shift + Enter`)
   * The query will return the results in the result grid at the bottom of the interface.

4. **Where Clause for Filtering Data**:

   * You can filter data using the **WHERE** clause. For example, if you want to retrieve only the customer with `customer_id` 1, use:

     ```sql
     SELECT * 
     FROM customers
     WHERE customer_id = 1;
     ```

5. **Order By Clause for Sorting Data**:

   * You can also **sort** the data using the **ORDER BY** clause. To sort the customers by their first name, you can use:

     ```sql
     SELECT * 
     FROM customers
     WHERE customer_id = 1
     ORDER BY first_name;
     ```
   * This query first filters the customer with `customer_id = 1` and then sorts the results by the `first_name` column.

6. **Commenting Out Code**:

   * You can use `--` to comment out any part of your query. For example:

     ```sql
     -- WHERE customer_id = 1;
     -- ORDER BY first_name;
     ```
   * This is useful if you want to temporarily disable a part of your query.

7. **Clause Order and Syntax**:

   * The order of clauses in a SQL statement is important. The correct order is:

     1. `SELECT`
     2. `FROM`
     3. `WHERE`
     4. `ORDER BY`
   * Reversing this order will result in a **syntax error**.

8. **Whitespace and Formatting**:

   * SQL ignores **whitespace**, **line breaks**, and **tabs**, so you can write SQL queries on a single line or across multiple lines.
   * However, it's a good practice to write each clause on a new line for clarity, especially in more complex queries.

---

### **Example SQL Query**

```sql
USE sql_store;

SELECT 
    * 
FROM 
    customers
WHERE 
    customer_id = 1
ORDER BY 
    first_name;
```

### **Key Points to Remember:**

* **Selecting a Database**: Use the `USE` statement or double-click a database to make it active.
* **Basic SELECT Syntax**: `SELECT` (columns) `FROM` (table).
* **WHERE Clause**: Filters data based on conditions.
* **ORDER BY Clause**: Sorts data by specified columns.
* **Correct Clause Order**: SQL statements must follow the proper order (SELECT → FROM → WHERE → ORDER BY).
* **Whitespace and Comments**: SQL ignores extra whitespace and line breaks, but it's good practice to format queries for readability.

---

## Queries from the video

```sql
USE sql_store;
```
```sql
SELECT 
    *
FROM
    customers
WHERE
    customer_id = 1
ORDER BY first_name;
```
