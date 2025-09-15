# 5 Creating the Databases

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/juEvrflxBWQ?si=R8ZKgNTYZkZq0TsV"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

> Join the discord server for downloadable content : [World of coders](https://discord.com/invite/NKwPMNzBTQ)

## 🛠️ Creating Databases in MySQL Workbench

### 🎥 Tutorial Overview

In this tutorial, we will:

1. **Understand the MySQL Workbench interface**.
2. **Create the necessary databases** for this course using SQL.
3. **Explore tables** in a database to understand how data is stored and related.

---

### **MySQL Workbench Interface Overview**

The **MySQL Workbench** interface might seem intimidating at first, but it's quite user-friendly once you get familiar with it. Here’s a breakdown of the interface:

* **Top Toolbar**:

  * Buttons for creating a new tab for writing SQL code, opening SQL files, creating databases, and creating new tables.

* **Left Panel (Navigator)**:

  * **Administration Tab**: Used for administrative tasks like managing the server, importing/exporting data, etc.
  * **Schemas Tab**: Displays the databases available in the MySQL server. The `CIS` database is the default MySQL system database.

* **Center Panel (Query Editor)**:

  * Where you'll write your SQL queries. This is the area we'll use most during the course.

* **Right Panel**:

  * **Context Help Tab**: Provides help and documentation.
  * **Snippets Tab**: Contains reusable SQL code snippets.

---

### **Step 1: Download and Prepare the SQL Files**

Before creating the databases, you’ll need to download a ZIP file that contains SQL files.

1. **Extract the ZIP file** and you’ll see several `.SQL` files, including the main one called `create_databases.sql`.
2. Open the `create_databases.sql` file in MySQL Workbench. This file contains all the necessary SQL code to create the databases required for this course.

---

### **Step 2: Execute the SQL Code to Create Databases**

Once the SQL file is open in MySQL Workbench, here’s how you execute the code to create databases:

1. **Click the yellow Thunder icon** (Execute) in the toolbar to run the SQL script.

   * If you want to execute just a part of the code, highlight the specific lines first.
   * If nothing is highlighted, the entire code will execute.

2. Check the **Output Window** at the bottom to see if the operation was successful. Each successful operation will show a **green message** indicating it completed without issues.

---

### **Step 3: Refresh the Schema View**

After executing the SQL script, the new databases should appear in the **Schemas Tab**. If you don’t see them, you may need to **refresh the schema view**:

1. **Click the refresh icon** in the Schemas panel to load the newly created databases.

---

### **Step 4: Explore the Databases**

In the **Schemas Tab**, you'll see databases prefixed with `SQL_`, such as `SQL_store`, `SQL_invoicing`, etc. These are the databases we'll use in this course.

1. **Example Database Exploration**:

   * Click to expand the `SQL_store` database.
   * In a database, we have several objects:

     * **Tables**: Where data is stored.
     * **Views**: Virtual tables that combine data from multiple tables, especially useful for reports.
     * **Stored Procedures/Functions**: Predefined operations or queries that can be executed on demand (e.g., retrieving all customers in a specific city).

2. **Tables in the `SQL_store` Database**:

   * Expand the **Tables** section to view the tables in the database (e.g., `customers`, `orders`, `products`).

---

### **Step 5: View Data in Tables**

To explore the data within a table:

1. **Open a Table**: Right-click on a table (e.g., `customers`), then click the **third icon** (the one with a table and a thunderbolt). This will open the data stored in that table.

2. **Understanding the Customer Table**:

   * Columns include `customer_id`, `first_name`, `last_name`, `birth_date`, `phone`, `address`, etc.
   * Each row represents a **record** (i.e., one customer).

3. **Example**:

   * **Customer Table**: The `customer_id` uniquely identifies each customer.
   * The `orders` table stores the orders placed by customers, where the `customer_id` is used to associate each order with a specific customer.

---

### **Why Use IDs Instead of Storing Data Directly?**

In relational databases, we don’t store redundant data. For example:

* In the **orders table**, instead of storing the **customer’s name, address, and phone number** for each order, we store the `customer_id`.
* This helps avoid redundancy. If a customer changes their address or phone number, we only need to update it in the **customers table**, not in every order they’ve placed.

---

### **Relational Databases and Table Relationships**

In relational databases, multiple tables are **linked together** using **relationships**. Here's an example:

* The `customer_id` in the `orders` table links to the `customer_id` in the `customers` table. This is called a **foreign key relationship**.

By using relationships, we can avoid redundant data and ensure that updates to information are centralized in one place.

---

### **Exercise: Explore the `invoicing` Database**

Before moving forward, explore the `invoicing` database. Spend a few minutes:

* Look at all the tables in the database.
* Understand the kind of data each table contains.
* Familiarize yourself with the relationships between tables.

This will help you get comfortable with how the databases are structured and prepare you for more complex queries in the future.

---

### **Summary**

| Step                            | Action                                                                 |
| ------------------------------- | ---------------------------------------------------------------------- |
| **1. Download SQL Files**       | Download and extract the `create_databases.sql` file.                  |
| **2. Execute SQL Code**         | Use the Thunder icon to execute the SQL code and create the databases. |
| **3. Refresh Schema View**      | Refresh the schemas to view the newly created databases.               |
| **4. Explore Databases**        | View tables, views, and stored procedures within the databases.        |
| **5. Understand Relationships** | Learn how different tables are related using foreign keys.             |