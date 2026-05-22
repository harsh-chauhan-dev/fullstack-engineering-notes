# Database Management Systems (DBMS) - Introduction

## Context

In modern software development, data is the most valuable asset. A Full Stack developer must understand not just how to build APIs, but how to store, retrieve, manipulate, and secure data efficiently.
This module covers the core concepts of Database Management Systems (DBMS), why we transitioned away from file systems, the architectures that power modern databases, and the core guarantees (ACID) that keep our data safe.

---

# 1. Understanding the Core Terms

Before diving into systems, let's define the fundamental terminology:

- **Data**: Raw facts, figures, or observations (e.g., `"John"`, `25`, `true`).
- **Information**: Processed, organized, and structured data that is meaningful (e.g., `"John is a 25-year-old active user"`).
- **Database**: An organized, systematic collection of structured data stored electronically in a computer system.
- **DBMS (Database Management System)**: The software package used to define, construct, manipulate, query, retrieve, and manage databases. It acts as an interface between the database and the end-users or application programs.
  - *Examples*: PostgreSQL, MySQL, MongoDB, Redis, SQLite.

---

# 2. File System vs. DBMS

Historically, data was stored in simple operating system files (e.g., CSV, TXT, custom binary formats). However, as systems grew, file-based storage became unmanageable.

| Feature | File System | DBMS |
| :--- | :--- | :--- |
| **Data Redundancy** | High (same data repeated in multiple files) | Low (redundancy minimized via Normalization) |
| **Data Inconsistency** | High (updating one file might leave another outdated) | Low (ensured via transaction constraints) |
| **Data Access** | Difficult (requires custom code to read each file format) | Easy (declarative languages like SQL or standardized APIs) |
| **Concurrent Access** | Poor (file locking prevents multiple writers) | Excellent (concurrency control via transactions/locks) |
| **Security & Privacy** | Low (basic OS file-level permissions) | High (fine-grained role-based access control, encryption) |
| **Data Integrity** | Hard to enforce (validation must be in application code) | Easy to enforce (foreign keys, check constraints, data types) |

---

# 3. The 3-Schema Architecture

To separate the physical database from user applications, ANSI-SPARC proposed the **Three-Schema Architecture** (also known as the Three-Level Architecture). This abstraction is critical for modern backend design.

### The Three Levels:
1. **External Level (View Schema)**:
   - The topmost level.
   - Defines how end-users or application APIs see the data.
   - Example: A student sees their grade, a teacher sees all grades, but neither sees how the grades are physically stored.
2. **Conceptual Level (Logical Schema)**:
   - The middle level.
   - Defines *what* data is stored and the relationships between data entities.
   - It represents the overall logical structure of the database (tables, schemas, column names, data types, constraints).
3. **Internal Level (Physical Schema)**:
   - The lowest level.
   - Defines *how* the data is physically stored on disk (block sizes, indexing methods like B-Trees, compression, file systems).

### Data Independence:
The main goal of this architecture is to achieve **Data Independence**:
- **Logical Data Independence**: The ability to modify the conceptual schema without changing the external schema (e.g., adding a new column to a table shouldn't break existing front-end views).
- **Physical Data Independence**: The ability to modify the physical schema without changing the conceptual schema (e.g., moving data from an SSD to an HDD, or changing index formats, without changing table structures or SQL queries).

---

# 4. Database Models: Relational vs. Non-Relational

Modern backend engineering utilizes different database models based on application requirements.

### A. Relational Database Management Systems (RDBMS)
- **Concept**: Data is organized into tables (Relations) made of rows (Tuples) and columns (Attributes).
- **Schema**: Rigid and predefined.
- **Language**: SQL (Structured Query Language).
- **Strengths**: Strong consistency, complex relationships, transactions.
- **Examples**: PostgreSQL, MySQL, SQLite, Microsoft SQL Server.

### B. Non-Relational Databases (NoSQL)
NoSQL databases are designed for high scalability, flexibility, and unstructured data. They fall into four main categories:
1. **Document-oriented**: Stores data in JSON-like documents (e.g., MongoDB, CouchDB).
2. **Key-Value**: Stores data as simple key-value pairs; extremely fast for caching (e.g., Redis, Memcached).
3. **Column-family (Wide-column)**: Stores data in columns instead of rows; optimized for massive write-heavy analytical queries (e.g., Apache Cassandra, ScyllaDB).
4. **Graph Databases**: Stores data in nodes and edges, optimized for highly interconnected data like social networks (e.g., Neo4j).

---

# 5. The ACID Properties

For any transactional backend system, ensuring data reliability is paramount. This is guaranteed by the **ACID** properties:

- **A - Atomicity ("All or Nothing")**:
  - A transaction consists of multiple operations. Either all of them succeed, or the entire transaction is rolled back as if nothing happened.
  - *Example*: In a bank transfer, deducting money from Account A and adding it to Account B must both succeed. If the system crashes mid-way, the money is returned to Account A.
- **C - Consistency**:
  - A transaction must transition the database from one valid state to another, upholding all constraints (e.g., unique keys, foreign keys, non-null values).
- **I - Isolation**:
  - Multiple concurrent transactions must execute without interfering with each other. The database should behave as if transactions run sequentially.
  - There are different isolation levels (Read Uncommitted, Read Committed, Repeatable Read, Serializable) which represent tradeoffs between performance and data correctness.
- **D - Durability**:
  - Once a transaction is committed, its changes are permanently recorded in non-volatile memory (disk) and will survive any subsequent system crash or power outage (achieved using Write-Ahead Logs / WAL).

---

# 6. Database Keys in Relational Systems

Keys uniquely identify records within a table and establish relationships between tables.

- **Super Key**: A set of one or more attributes that can uniquely identify a row in a table.
- **Candidate Key**: A minimal Super Key (no redundant attributes). Any candidate key is eligible to become the Primary Key.
- **Primary Key**: A chosen Candidate Key that uniquely identifies each row in a table. It cannot be null and must be unique.
- **Foreign Key**: An attribute in a table that references the Primary Key of another table. It is used to enforce Referential Integrity between tables.

---

# 7. Sub-Languages of SQL

When working with RDBMS, you write queries using SQL. SQL commands are categorized into sub-languages based on their actions:

1. **DDL (Data Definition Language)**: Defines/modifies the database structure.
   - `CREATE`, `ALTER`, `DROP`, `TRUNCATE`
2. **DML (Data Manipulation Language)**: Modifies/queries the actual data.
   - `SELECT`, `INSERT`, `UPDATE`, `DELETE`
3. **DCL (Data Control Language)**: Manages permissions and security.
   - `GRANT`, `REVOKE`
4. **TCL (Transaction Control Language)**: Manages database transactions.
   - `COMMIT`, `ROLLBACK`, `SAVEPOINT`

---

# 8. Practical Observation Task

To get hands-on experience with how databases handle relational concepts, let's look at a simple relational schema design.

Suppose we have two entities: **Users** and **Orders**.

### 1. The Users Table (`users`)
| user_id (PK) | username | email |
| :--- | :--- | :--- |
| 1 | user_harsh | harsh@example.com |
| 2 | dev_john | john@example.com |

### 2. The Orders Table (`orders`)
| order_id (PK) | user_id (FK) | total_amount | order_date |
| :--- | :--- | :--- | :--- |
| 101 | 1 | 250.00 | 2026-05-23 |
| 102 | 1 | 45.50 | 2026-05-23 |
| 103 | 2 | 120.00 | 2026-05-24 |

**Task**: Notice how `user_id` in the `orders` table acts as a **Foreign Key** referencing `user_id` in the `users` table. This prevents an order from being created for a user that does not exist (Referential Integrity).

---

# Interview Questions

1. Why do we use a DBMS instead of storing data directly in local files (File System)?
2. What are the three levels of database architecture, and what is physical vs. logical data independence?
3. Explain the difference between Relational (SQL) and Non-Relational (NoSQL) databases. When would you choose one over the other?
4. What are the ACID properties in a database? Provide a real-world scenario where Atomicity is crucial.
5. What is the difference between a Candidate Key, a Primary Key, and a Foreign Key?
6. What is the purpose of a Foreign Key constraint, and what is Referential Integrity?
7. Group the following SQL commands into their appropriate sub-languages (DDL, DML, TCL): `SELECT`, `ALTER`, `COMMIT`, `CREATE`, `DELETE`, `ROLLBACK`.

---

# Summary

- **DBMS** provides structured, safe, and concurrent access to data, resolving the redundancy and inconsistency issues of traditional **File Systems**.
- **Three-Level Architecture** abstracts the physical storage details away from the logical schema and user views, providing **Data Independence**.
- **Relational Databases** use highly structured tables with strict schemas and SQL, while **NoSQL** databases provide high flexibility and scalability.
- **ACID properties** are the foundation of transaction management, guaranteeing data integrity even during system failures.
- **Keys** (Primary, Candidate, Foreign) form the logical blueprint of data relations and maintain consistency.
