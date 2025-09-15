# 2 What is SQL

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/9fqme5-CdmA?si=IjAaFAZylZKGbipT"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

### 🗃️ What is a Database?

> *"A database is a collection of data stored in a format that can easily be accessed."*

* A **Database** is a structured collection of data.
* It allows us to **store**, **retrieve**, and **manipulate** data efficiently.
* Databases are managed using software called a **DBMS** (Database Management System).

---

### 🧠 What is a DBMS?

> *"To manage our databases, we use a software application called a database management system or DBMS."*

* A **DBMS** is a tool that lets us interact with databases.
* It receives **instructions** (queries) from us and executes them.
* Examples:

  * MySQL
  * SQL Server
  * Oracle
  * PostgreSQL

---

### 🔍 Types of Databases

Databases fall into two main categories:

| Type                       | Description                                                                                     |
| -------------------------- | ----------------------------------------------------------------------------------------------- |
| **Relational (RDBMS)**     | Data is stored in **tables** with predefined **relationships** (e.g., Customer, Product, Order) |
| **Non-Relational (NoSQL)** | Data is stored in **non-tabular** formats (e.g., JSON, key-value pairs, graphs)                 |

---

### 📋 Relational Databases (RDBMS)

> *"In relational databases we store data in tables that are linked to each other using relationships."*

* Data is organized in **tables** (rows and columns).
* Tables can be **linked** through relationships (foreign keys).
* We use **SQL** to interact with relational databases.

---

### 🧾 What is SQL?

> *"SQL or S-Q-L is the language that we use to work with relational database management systems."*

* Stands for **Structured Query Language**
* Used to:

  * **Query** data: `SELECT`
  * **Modify** data: `INSERT`, `UPDATE`, `DELETE`
  * **Define** schema: `CREATE`, `ALTER`, `DROP`

✅ **Example SQL Query:**

```sql
SELECT * FROM customers;
```

* This command fetches all records from the `customers` table.

---

### 🛠️ Popular Relational Database Systems

| DBMS            | Developer                       | Notes                           |
| --------------- | ------------------------------- | ------------------------------- |
| MySQL           | Oracle (originally by MySQL AB) | Most popular open-source DBMS   |
| SQL Server      | Microsoft                       | Strong integration with Windows |
| Oracle Database | Oracle Corp.                    | Widely used in enterprises      |
| PostgreSQL      | Open Source                     | Advanced SQL features           |

All of these use **SQL**, but with slightly different "flavors" or implementations. The majority of standard SQL works across all of them.

---

### 🐬 Why MySQL?

> *"In this course we'll be using MySQL, which is the most popular open-source database in the world."*

* **MySQL** is widely adopted and great for beginners.
* Compatible with many platforms.
* Free and open-source.

---

### 🗣️ How Do You Pronounce SQL?

> *"As you talk to different people you will hear two different pronunciations of SQL: 'ess-cue-ell' or 'sequel'."*

* **SQL** was originally called **SEQUEL** (Structured English Query Language).
* Renamed due to trademark issues.
* Both pronunciations are acceptable:

  * **"Sequel"** – Common in North America
  * **"S-Q-L"** – Common globally

Instructor’s Preference:

* He says **"Sequel"** (SQL), but both are fine.

---

### 🗣️ How Do You Pronounce MySQL?

> *"Developers of this product prefer to call it 'My S-Q-L' rather than 'My Sequel'."*

* Preferred: **"My S-Q-L"**
* But: Most developers won’t mind either way

---

## ✅ Summary

* A **database** stores and organizes data.
* A **DBMS** allows us to manage databases.
* **SQL** is the language used to work with relational databases.
* We'll use **MySQL** in this course.
* Both **SQL** and **MySQL** have flexible pronunciations.