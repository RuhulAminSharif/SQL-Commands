<h1> SQL-Commands </h1>

This repository is a comprehensive collection of basic SQL/PostgreSQL commands. Here's a sample README file based on the topics you've covered in PostgreSQL Basics:

---
## Table of Contents
Here is the list of topics that I have covered till now:
- [Table of Contents](#table-of-contents)
  - [1. Managing Databases](#1-managing-databases)
  - [2. Execution Order](#2-execution-order)
  - [3. Querying Data](#3-querying-data)
  - [4. Filtering Data](#4-filtering-data)
  - [5. Joining Tables](#5-joining-tables)
  - [6. Aggregate Functions](#6-aggregate-functions)
  - [7. Operators](#7-operators)
  - [8. Grouping Data](#8-grouping-data)
  - [9. Set Operations](#9-set-operations)
  - [10. Sub-queries](#10-sub-queries)
  - [11. Common Table Expressions (CTE)](#11-common-table-expressions-cte)
  - [12. Modifying Data](#12-modifying-data)
  - [13. Transactions](#13-transactions)
  - [14. Managing Tables](#14-managing-tables)
  - [15. Constraints](#15-constraints)
  - [16. Conditional Expressions \& Operators](#16-conditional-expressions--operators)
  - [17. Future Topics](#17-future-topics)

---

### 1. Managing Databases
- **Commands**: `SHOW`, `CREATE`, `DROP`, `BACKUP`, `RESTORE`, `RENAME`, `COPY`
- **Scope**: Database management

### 2. Execution Order
- **Commands**: `SELECT` (with all clauses)
- **Scope**: Query order of operations in SQL

### 3. Querying Data
- **Commands**: `SELECT`, `SELECT DISTINCT`, Column Aliases
- **Additional**: Ordering data with `ORDER BY`

### 4. Filtering Data
- **Clauses**: 
  - `WHERE`
  - `AND`, `OR`
  - `LIMIT`, `OFFSET`, `FETCH`
  - `IN`, `BETWEEN`
  - `LIKE`, `ILIKE`
  - `IS NULL`

### 5. Joining Tables
- **Types of Joins**:
  - `TABLE ALIAS`
  - `INNER JOIN`, `FULL OUTER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `SELF JOIN`, `CROSS JOIN`, `NATURAL JOIN`

### 6. Aggregate Functions
- **Functions**: `COUNT`, `MAX`, `MIN`, `SUM`, `AVG`

### 7. Operators
- **Types**:
  - Logical
  - Arithmetic
  - Comparison

### 8. Grouping Data
- **Commands**: `GROUP BY`, `HAVING`
- **Grouping Sets**: `GROUPING SETS`, `CUBE`, `ROLLUP`

### 9. Set Operations
- **Commands**: `UNION`, `INTERSECT`, `EXCEPT`

### 10. Sub-queries
- **Sub-query Types**: `ANY/SOME`, `ALL`, `EXISTS`

### 11. Common Table Expressions (CTE)
- **Command**: `WITH`

### 12. Modifying Data
- **Commands**:
  - Inserting: `INSERT INTO`
  - Updating: `UPDATE`
  - Deleting: `DELETE`
  - Conflict Handling: `UPSERT`, `ON CONFLICT`

### 13. Transactions
- **Commands**: `BEGIN`, `COMMIT`, `ROLLBACK`

### 14. Managing Tables
- **Commands**:
  - Creating: `CREATE TABLE`, `CREATE TABLE AS`
  - Selecting: `SELECT INTO`
  - Auto-Increment: `SERIAL`
  - Altering: `ALTER TABLE`

### 15. Constraints
- **Types**:
  - `PRIMARY KEY`, `FOREIGN KEY`
  - `CHECK`, `UNIQUE`, `NOT NULL`, `DEFAULT`

### 16. Conditional Expressions & Operators
- **Commands**:
  - `CASE`
  - `COALESCE`
  - `NULLIF`
  - `CAST`
### 17. Future Topics
In future updates, this guide will cover additional PostgreSQL topics:
- **Aggregate Functions** (Advanced usage)
- **Date Functions**
- **JSON Functions**
- **Math Functions**
- **String Functions**
- **Window Functions**
- **PostgreSQL Views**
- **PostgreSQL Indexes**
- **PostgreSQL Trigger Basics**
- **PostgreSQL Administration**
---

This README provides a quick reference to essential PostgreSQL commands for database management, data querying, and data manipulation. Refer to the PostgreSQL documentation for more detailed syntax and examples.