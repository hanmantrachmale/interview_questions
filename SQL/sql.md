# .NET (SQL) Interview Questions and Answers

## SQL
1. **What is the difference between INNER JOIN, LEFT JOIN, RIGHT JOIN, and FULL JOIN?**  
2. **What is a primary key and how does it differ from a unique key?**  
3. **What are foreign keys and how do they enforce referential integrity?**  
4. **Explain normalization and list the different normal forms.**  
5. **What is a clustered index vs a non-clustered index?**  
6. **What are transactions in SQL and what are ACID properties?**  
7. **What is the difference between DELETE, TRUNCATE, and DROP?**  
8. **What are window functions in SQL and when would you use them?**  
9. **How does a Common Table Expression (CTE) work and how is it different from a subquery?**  
10. **What are the advantages and disadvantages of using stored procedures?**  
11. **How can you detect and prevent SQL injection?**  
12. **What is the difference between EXISTS and IN operators in SQL?**  
13. **How does indexing work and how can you identify slow queries?**  
14. **What is the use of the `EXPLAIN` or `QUERY PLAN` statement?**  
15. **What are aggregate functions and how are GROUP BY and HAVING used?**  
16. **What is a composite key and when should it be used?**  
17. **What is a materialized view and how does it differ from a regular view?**  
18. **How do you handle NULL values in queries and constraints?**  
19. **What is the difference between scalar functions and table-valued functions?**  
20. **How would you design a schema for a multi-tenant application in SQL?**  

## 1. **What is the difference between INNER JOIN, LEFT JOIN, RIGHT JOIN, and FULL JOIN?**

**Answer:**  
These SQL join types determine how rows from two tables are combined based on a related column.

- **INNER JOIN**: Returns only rows where there is a match in both tables.
- **LEFT JOIN**: Returns all rows from the left table and matching rows from the right. NULLs if no match.
- **RIGHT JOIN**: Returns all rows from the right table and matching rows from the left. NULLs if no match.
- **FULL JOIN**: Returns all rows when there is a match in either table. NULLs where no match exists.

---

### Example Tables

**Customers**

| Id | Name     |
|----|----------|
| 1  | Alice    |
| 2  | Bob      |
| 3  | Charlie  |

**Orders**

| Id | CustomerId | Product   |
|----|------------|-----------|
| 1  | 1          | Keyboard  |
| 2  | 1          | Mouse     |
| 3  | 2          | Monitor   |
| 4  | 4          | Webcam    |

---

### INNER JOIN

```sql
SELECT Customers.Name, Orders.Product
FROM Customers
INNER JOIN Orders ON Customers.Id = Orders.CustomerId;
```

**Result:**

| Name  | Product  |
|-------|----------|
| Alice | Keyboard |
| Alice | Mouse    |
| Bob   | Monitor  |

---

### LEFT JOIN

```sql
SELECT Customers.Name, Orders.Product
FROM Customers
LEFT JOIN Orders ON Customers.Id = Orders.CustomerId;
```

**Result:**

| Name    | Product  |
|---------|----------|
| Alice   | Keyboard |
| Alice   | Mouse    |
| Bob     | Monitor  |
| Charlie | NULL     |

---

### RIGHT JOIN

```sql
SELECT Customers.Name, Orders.Product
FROM Customers
RIGHT JOIN Orders ON Customers.Id = Orders.CustomerId;
```

**Result:**

| Name  | Product  |
|-------|----------|
| Alice | Keyboard |
| Alice | Mouse    |
| Bob   | Monitor  |
| NULL  | Webcam   |

---

### FULL JOIN

```sql
SELECT Customers.Name, Orders.Product
FROM Customers
FULL OUTER JOIN Orders ON Customers.Id = Orders.CustomerId;
```

**Result:**

| Name    | Product  |
|---------|----------|
| Alice   | Keyboard |
| Alice   | Mouse    |
| Bob     | Monitor  |
| Charlie | NULL     |
| NULL    | Webcam   |

--- 

## 2. **What is a primary key and how does it differ from a unique key?**

**Answer:**  
A **primary key** is a column or combination of columns that uniquely identifies each row in a table. It ensures that:

- Values are **unique** across rows.
- Values are **not NULL**.
- Each table can only have **one primary key**.

A **unique key** also ensures uniqueness of values, but:

- It **can allow NULLs** (depending on the database system).
- A table can have **multiple unique keys**.
- It is typically used to enforce business rules, like ensuring email or username is unique.

---

### Example: Primary Key vs Unique Key

```sql
CREATE TABLE Employees (
    EmployeeId INT PRIMARY KEY,          -- Primary key
    Email NVARCHAR(100) UNIQUE,          -- Unique key
    SSN CHAR(9) UNIQUE,                  -- Another unique key
    FullName NVARCHAR(100)
);
```

**Explanation:**
- `EmployeeId` is the primary key — must be unique and cannot be NULL.
- `Email` and `SSN` must also be unique, but can be NULL (depends on the RDBMS).
- You can have only one primary key, but multiple unique keys.

---

### Comparison Table

| Feature              | Primary Key        | Unique Key         |
|----------------------|--------------------|---------------------|
| Uniqueness           | ✅ Ensured          | ✅ Ensured           |
| NULLs allowed        | ❌ Not allowed      | ✅ Allowed (usually) |
| Number per table     | 1 only              | Multiple allowed     |
| Default index type   | Clustered (default) | Non-clustered (usually) |
| Used for             | Row identity        | Business rules       |

---

## 3. **What are foreign keys and how do they enforce referential integrity?**

**Answer:**  
A **foreign key** is a constraint used to link two tables together. It enforces **referential integrity** by ensuring that the value in one table (child) corresponds to a valid value in another table (parent).

- It prevents inserting invalid references.
- It restricts deleting parent rows that are referenced by child rows (unless cascading is enabled).

---

### Example

```sql
-- Parent table
CREATE TABLE Customers (
    Id INT PRIMARY KEY,
    Name NVARCHAR(100)
);

-- Child table with a foreign key
CREATE TABLE Orders (
    Id INT PRIMARY KEY,
    CustomerId INT,
    Product NVARCHAR(100),
    FOREIGN KEY (CustomerId) REFERENCES Customers(Id)
);
```

This ensures that every `CustomerId` in `Orders` must exist in `Customers`.

---

### Benefits

- Maintains consistency between related tables.
- Helps avoid orphaned records.
- Supports cascading operations (`ON DELETE`, `ON UPDATE`).

---

## 4. **Explain normalization and list the different normal forms.**

**Answer:**  
**Normalization** is the process of organizing data in a database to reduce **redundancy** and improve **data integrity**.

Each level of normalization is called a **normal form** (NF):

- **1NF (First Normal Form)**  
  - Ensures each column contains atomic (indivisible) values.  
  - No repeating groups or arrays in rows.

- **2NF (Second Normal Form)**  
  - Must be in 1NF.  
  - All non-key attributes are fully functionally dependent on the **entire** primary key (eliminates partial dependency).

- **3NF (Third Normal Form)**  
  - Must be in 2NF.  
  - No transitive dependency (non-key attributes depend only on the key, not on other non-key attributes).

---

### Example

#### Unnormalized Table (Repeating groups):

| OrderId | Customer | Products         |
|---------|----------|------------------|
| 1       | Alice    | Mouse, Keyboard  |

#### 1NF (Atomic values):

| OrderId | Customer | Product   |
|--------|----------|-----------|
| 1      | Alice    | Mouse     |
| 1      | Alice    | Keyboard  |

#### 2NF (Split into Orders and Customers):

**Customers**

| CustomerId | Name   |
|------------|--------|
| 1          | Alice  |

**Orders**

| OrderId | CustomerId | Product   |
|---------|------------|-----------|
| 1       | 1          | Mouse     |
| 2       | 1          | Keyboard  |

---

### Why Normalize?

- Eliminates data duplication.
- Reduces anomalies in insert/update/delete.
- Improves data integrity and maintainability.

---

## 5. **What is a clustered index vs a non-clustered index?**

**Answer:**  
Indexes improve data retrieval speed in SQL. There are two main types:

- **Clustered Index**
  - Determines the **physical order** of rows in the table.
  - A table can have **only one** clustered index.
  - Data is **stored in the same order** as the clustered index.
  - Typically created on the **primary key**.

- **Non-Clustered Index**
  - Contains a pointer to the actual data row.
  - A table can have **multiple** non-clustered indexes.
  - Does **not affect** the physical order of data.

---

### Example

```sql
-- Clustered index (usually created by default on primary key)
CREATE TABLE Employees (
    Id INT PRIMARY KEY,       -- clustered index
    Name NVARCHAR(100),
    Email NVARCHAR(100)
);

-- Non-clustered index
CREATE NONCLUSTERED INDEX IX_Employees_Email
ON Employees (Email);
```

---

### Comparison Table

| Feature             | Clustered Index       | Non-Clustered Index     |
|---------------------|------------------------|--------------------------|
| Physical sort order | Yes                    | No                       |
| Number per table    | One                    | Many                     |
| Storage             | Table data itself      | Separate index structure |
| Access speed        | Faster for range queries | Good for lookups         |

---

## 6. **What are transactions in SQL and what are ACID properties?**

**Answer:**  
A **transaction** is a sequence of operations performed as a **single logical unit of work**. Transactions follow the **ACID** properties to ensure data integrity.

---

### ACID Properties

- **Atomicity**: All operations succeed or none at all.
- **Consistency**: Brings the database from one valid state to another.
- **Isolation**: Transactions run independently of each other.
- **Durability**: Once committed, the results are permanent, even in the case of a crash.

---

### Example

```sql
BEGIN TRANSACTION;

UPDATE Accounts SET Balance = Balance - 100 WHERE Id = 1;
UPDATE Accounts SET Balance = Balance + 100 WHERE Id = 2;

-- Commit changes only if both succeed
COMMIT;

-- If something goes wrong
-- ROLLBACK;
```

---

### Why Use Transactions?

- Prevents partial updates.
- Ensures consistency during concurrent operations.
- Essential in financial and critical data operations.

---

## 7. **What is the difference between DELETE, TRUNCATE, and DROP?**

**Answer:**  
These SQL commands are used to remove data, but they differ in scope and behavior:

- **DELETE**
  - Removes rows **one at a time** based on a `WHERE` clause.
  - **Can be rolled back** if inside a transaction.
  - **Fires triggers** (if defined).
  - **Slower** for large datasets.

- **TRUNCATE**
  - Removes **all rows** from a table instantly.
  - Cannot use `WHERE`.
  - **Minimal logging**, so it’s much **faster**.
  - Cannot be rolled back in some RDBMS (e.g., MySQL); in others (like SQL Server), it can be if within a transaction.
  - **Does not fire** triggers.

- **DROP**
  - Completely **removes the table or object** from the database.
  - Deletes the structure, data, indexes, constraints — everything.
  - **Cannot be rolled back**.

---

### Example

```sql
-- DELETE specific records
DELETE FROM Employees WHERE Department = 'Sales';

-- TRUNCATE all records
TRUNCATE TABLE Employees;

-- DROP the table entirely
DROP TABLE Employees;
```

---

### Comparison Table

| Feature          | DELETE             | TRUNCATE           | DROP              |
|------------------|--------------------|--------------------|-------------------|
| Removes data     | ✅ Row-by-row       | ✅ All at once      | ✅ Entire table    |
| WHERE supported  | ✅ Yes              | ❌ No              | ❌ No              |
| Rollback support | ✅ Yes              | ⚠️ Depends on RDBMS | ❌ No              |
| Fires triggers   | ✅ Yes              | ❌ No              | ❌ No              |
| Affects structure| ❌ No               | ❌ No               | ✅ Yes             |

---

## 8. **What are window functions in SQL and when would you use them?**

**Answer:**  
**Window functions** perform calculations across a set of rows that are **related to the current row**, without collapsing rows like `GROUP BY`.

They are useful for:
- Ranking
- Running totals
- Moving averages
- Percentiles

---

### Common Window Functions

- `ROW_NUMBER()`
- `RANK()`
- `DENSE_RANK()`
- `SUM() OVER()`
- `AVG() OVER()`
- `LAG() / LEAD()`

---

### Example

```sql
SELECT
    EmployeeId,
    Department,
    Salary,
    RANK() OVER (PARTITION BY Department ORDER BY Salary DESC) AS SalaryRank
FROM Employees;
```

**Explanation:**
- Ranks employees by salary **within each department**.
- Does not group rows — every row is retained.

---

### Use Cases

- Paginating results (`ROW_NUMBER()` with `OFFSET`)
- Top-N queries per category
- Comparing a row to the previous or next row (`LAG`, `LEAD`)
- Cumulative totals

--- 

## 7. **What is the difference between DELETE, TRUNCATE, and DROP?**

**Answer:**  
These SQL commands are used to remove data, but they differ in scope and behavior:

- **DELETE**
  - Removes rows **one at a time** based on a `WHERE` clause.
  - **Can be rolled back** if inside a transaction.
  - **Fires triggers** (if defined).
  - **Slower** for large datasets.

- **TRUNCATE**
  - Removes **all rows** from a table instantly.
  - Cannot use `WHERE`.
  - **Minimal logging**, so it’s much **faster**.
  - Cannot be rolled back in some RDBMS (e.g., MySQL); in others (like SQL Server), it can be if within a transaction.
  - **Does not fire** triggers.

- **DROP**
  - Completely **removes the table or object** from the database.
  - Deletes the structure, data, indexes, constraints — everything.
  - **Cannot be rolled back**.

---

### Example

```sql
-- DELETE specific records
DELETE FROM Employees WHERE Department = 'Sales';

-- TRUNCATE all records
TRUNCATE TABLE Employees;

-- DROP the table entirely
DROP TABLE Employees;
```

---

### Comparison Table

| Feature          | DELETE             | TRUNCATE           | DROP              |
|------------------|--------------------|--------------------|-------------------|
| Removes data     | ✅ Row-by-row       | ✅ All at once      | ✅ Entire table    |
| WHERE supported  | ✅ Yes              | ❌ No              | ❌ No              |
| Rollback support | ✅ Yes              | ⚠️ Depends on RDBMS | ❌ No              |
| Fires triggers   | ✅ Yes              | ❌ No              | ❌ No              |
| Affects structure| ❌ No               | ❌ No               | ✅ Yes             |

---

## 8. **What are window functions in SQL and when would you use them?**

**Answer:**  
**Window functions** perform calculations across a set of rows that are **related to the current row**, without collapsing rows like `GROUP BY`.

They are useful for:
- Ranking
- Running totals
- Moving averages
- Percentiles

---

### Common Window Functions

- `ROW_NUMBER()`
- `RANK()`
- `DENSE_RANK()`
- `SUM() OVER()`
- `AVG() OVER()`
- `LAG() / LEAD()`

---

### Example

```sql
SELECT
    EmployeeId,
    Department,
    Salary,
    RANK() OVER (PARTITION BY Department ORDER BY Salary DESC) AS SalaryRank
FROM Employees;
```

**Explanation:**
- Ranks employees by salary **within each department**.
- Does not group rows — every row is retained.

---

### Use Cases

- Paginating results (`ROW_NUMBER()` with `OFFSET`)
- Top-N queries per category
- Comparing a row to the previous or next row (`LAG`, `LEAD`)
- Cumulative totals

---

## 9. **How does a Common Table Expression (CTE) work and how is it different from a subquery?**

**Answer:**  
A **Common Table Expression (CTE)** is a named temporary result set defined using the `WITH` keyword. It improves readability and can reference itself (recursive CTEs).

---

### Syntax

```sql
WITH CTE_Name AS (
    SELECT Column1, Column2
    FROM SomeTable
    WHERE Condition
)
SELECT * FROM CTE_Name;
```

---

### Benefits of CTEs

- Improves query **readability**.
- Can be **recursive**.
- Makes complex logic easier to structure.
- Reusable within the same query (unlike subqueries which are repeated).

---

### Subquery vs CTE

| Feature            | Subquery              | CTE                         |
|--------------------|-----------------------|-----------------------------|
| Readability        | Can get messy         | Cleaner and more readable  |
| Reuse              | Cannot be reused      | Can be referenced multiple times |
| Recursion support  | ❌ No                 | ✅ Yes                     |
| Performance        | Similar in most cases | Similar in most cases       |

---

### Example – CTE vs Subquery

**CTE version:**

```sql
WITH SalesAboveAvg AS (
    SELECT * FROM Sales
    WHERE Amount > (SELECT AVG(Amount) FROM Sales)
)
SELECT * FROM SalesAboveAvg;
```

**Subquery version:**

```sql
SELECT * FROM Sales
WHERE Amount > (SELECT AVG(Amount) FROM Sales);
```

---

## 10. **What are the advantages and disadvantages of using stored procedures?**

**Answer:**  
**Stored procedures** are precompiled SQL statements stored in the database, often used to encapsulate business logic.

---

### Advantages

- **Performance**: Precompiled execution = faster for repeated calls.
- **Security**: Can restrict access to underlying tables.
- **Reusability**: Encapsulate logic in one place.
- **Maintainability**: Logic changes don’t require application recompilation.

---

### Disadvantages

- **Complexity**: Business logic may be scattered between code and DB.
- **Debugging**: Harder to debug than application logic.
- **Portability**: Tied to a specific RDBMS (T-SQL vs PL/pgSQL etc.).
- **Versioning**: Not as easy to track in source control.

---

### Example

```sql
CREATE PROCEDURE GetTopSellingProducts
    @MinQuantity INT
AS
BEGIN
    SELECT ProductName, SUM(Quantity) AS TotalSold
    FROM OrderDetails
    GROUP BY ProductName
    HAVING SUM(Quantity) > @MinQuantity;
END;
```

```sql
-- Execute the procedure
EXEC GetTopSellingProducts @MinQuantity = 50;
```

---

## 11. **How can you detect and prevent SQL injection?**

**Answer:**  
**SQL injection** is a security vulnerability where attackers inject malicious SQL code through user input. To detect and prevent it:

---

### Prevention Techniques

- ✅ **Use parameterized queries / prepared statements**  
- ✅ **Use ORM frameworks** (e.g., Entity Framework) which handle parameters internally  
- ✅ **Validate and sanitize user input**  
- ✅ **Least privilege principle** — avoid excessive DB permissions  
- ✅ **Avoid dynamic SQL** unless necessary, and always parameterize it  
- ✅ **Use stored procedures** with parameters

---

### Example — Unsafe (vulnerable to SQL injection):

```csharp
// DANGEROUS! Never concatenate user input into SQL
var query = "SELECT * FROM Users WHERE Username = '" + username + "'";
```

---

### Example — Safe (parameterized):

```csharp
var query = "SELECT * FROM Users WHERE Username = @username";
var command = new SqlCommand(query, connection);
command.Parameters.AddWithValue("@username", username);
```

---

### Example — EF Core Safe Interpolation:

```csharp
var user = await context.Users
    .FromSqlInterpolated($"SELECT * FROM Users WHERE Username = {username}")
    .FirstOrDefaultAsync();
```

---

## 12. **What is the difference between EXISTS and IN operators in SQL?**

**Answer:**  
Both `EXISTS` and `IN` are used to filter query results based on another set of results, but they work differently:

---

### `IN` operator:

- Compares a value to a **list of values** returned by a subquery.
- Subquery is evaluated and loaded into memory first.
- Suitable when working with **small to medium** result sets.

---

### `EXISTS` operator:

- Checks for **existence of rows** returned by a subquery.
- Returns **true immediately** when a match is found.
- Often faster with **large or correlated** datasets.

---

### Example — IN

```sql
SELECT Name
FROM Customers
WHERE Id IN (SELECT CustomerId FROM Orders);
```

---

### Example — EXISTS

```sql
SELECT Name
FROM Customers c
WHERE EXISTS (
    SELECT 1 FROM Orders o WHERE o.CustomerId = c.Id
);
```

### Performance Consideration

| Criteria             | IN                         | EXISTS                         |
|----------------------|----------------------------|---------------------------------|
| Use case             | Value comparison           | Existence check                |
| Subquery execution   | All results loaded         | Stops at first match           |
| Works with NULLs     | Can be problematic         | ✅ Handles NULLs gracefully     |
| Ideal for            | Small datasets             | Large datasets / correlated queries |

---

## 13. **How does indexing work and how can you identify slow queries?**

**Answer:**  
An **index** is a database structure that improves the speed of data retrieval operations on a table at the cost of additional storage and slower write operations (INSERT, UPDATE, DELETE).

---

### How Indexing Works

- Works like a **lookup table** for faster data access.
- **B-tree structure** is most common.
- Indexes store a sorted list of column values and pointers to their corresponding rows.

---

### Types of Indexes

- **Clustered Index**: Sorts the physical data in the table (only one per table).
- **Non-Clustered Index**: Stores index separately from table data (many allowed).
- **Composite Index**: Index on multiple columns.
- **Covering Index**: Contains all columns needed by a query.

---

### Identifying Slow Queries

- Use **query execution plans** to analyze performance.
- Look for **table scans** or **missing index warnings**.
- Tools to use:
  - SQL Server: **Execution Plan**, **SQL Profiler**
  - PostgreSQL: **EXPLAIN ANALYZE**
  - MySQL: **EXPLAIN**

---

### Example — Creating Index

```sql
-- Create index on LastName column
CREATE INDEX IX_Employees_LastName ON Employees(LastName);
```

---

## 14. **What is the use of the `EXPLAIN` or `QUERY PLAN` statement?**

**Answer:**  
`EXPLAIN` (or `EXPLAIN ANALYZE`) shows the **query execution plan**, which details how the database engine will execute a SQL query.

---

### What It Helps With

- Reveals **which indexes are used**
- Shows **join algorithms** (nested loop, hash join, etc.)
- Identifies **bottlenecks** like full table scans
- Estimates **row counts and costs**

---

### Syntax Examples

**PostgreSQL:**

```sql
EXPLAIN ANALYZE
SELECT * FROM Orders WHERE CustomerId = 5;
```

**SQL Server:**

```sql
-- Enable actual execution plan
-- Ctrl + M in SSMS before running query
SELECT * FROM Orders WHERE CustomerId = 5;
```

**MySQL:**

```sql
EXPLAIN
SELECT * FROM Orders WHERE CustomerId = 5;
```

---

### Example Output Highlights

| Field        | Meaning                            |
|--------------|-------------------------------------|
| Seq Scan     | Sequential (table) scan occurred   |
| Index Scan   | Index was used                     |
| Rows         | Estimated number of rows processed |
| Cost         | Estimated time/memory cost         |

---

### Why It's Useful

- Helps you **optimize queries** and index usage.
- Essential for troubleshooting **performance issues**.

---

## 15. **What are aggregate functions and how are GROUP BY and HAVING used?**

**Answer:**  
**Aggregate functions** perform calculations on a set of values and return a single value. Common aggregate functions include:

- `COUNT()` – total number of rows
- `SUM()` – total value
- `AVG()` – average value
- `MIN()` – minimum value
- `MAX()` – maximum value

---

### GROUP BY

- Groups rows that have the same values in specified columns into summary rows.
- Used with aggregate functions to produce grouped results.

### HAVING

- Filters the results **after** grouping has been applied.
- Similar to `WHERE`, but works with aggregate values.

---

### Example

```sql
SELECT Department, COUNT(*) AS EmployeeCount
FROM Employees
GROUP BY Department
HAVING COUNT(*) > 5;
```

**Explanation:**
- Groups employees by department.
- Only shows departments with more than 5 employees.

---

### GROUP BY vs HAVING vs WHERE

| Clause  | Filters Before/After Aggregation | Can Use Aggregates? |
|---------|-------------------------------|----------------------|
| WHERE   | Before                        | ❌ No                |
| HAVING  | After                         | ✅ Yes               |
| GROUP BY| N/A (organizes data)          | N/A                 |

---

## 16. **What is a composite key and when should it be used?**

**Answer:**  
A **composite key** is a combination of two or more columns used as the **primary key** of a table. Together, these columns **uniquely identify** each row.

---

### When to Use a Composite Key

- No single column is unique by itself.
- A combination of multiple columns guarantees uniqueness.
- Often used in **many-to-many** relationship tables (junction tables).

---

### Example

```sql
CREATE TABLE CourseEnrollments (
    StudentId INT,
    CourseId INT,
    EnrolledOn DATE,
    PRIMARY KEY (StudentId, CourseId)
);
```

**Explanation:**
- A student can enroll in multiple courses.
- A course can have multiple students.
- The combination of `StudentId` and `CourseId` uniquely identifies each enrollment.

---

### Benefits

- Prevents duplicate entries across multiple fields.
- Enforces real-world uniqueness constraints.
- Saves space compared to using surrogate keys (sometimes).

---

### Caveats

- Can make joins more complex.
- Indexing on multiple columns may require fine-tuning for performance.

---

## 17. **What is a materialized view and how does it differ from a regular view?**

**Answer:**  
A **materialized view** is a database object that stores the result of a query **physically on disk**, unlike a regular view which is a **virtual** table that runs the query each time it is accessed.

---

### Key Differences

| Feature              | View                       | Materialized View           |
|----------------------|----------------------------|-----------------------------|
| Storage              | No (virtual)               | Yes (stored on disk)        |
| Performance          | Slower (recomputes)        | Faster (precomputed)        |
| Data freshness       | Always up-to-date          | Can be stale (requires refresh) |
| Use case             | Lightweight abstraction    | Optimized for heavy queries |

---

### Example – Regular View

```sql
CREATE VIEW ActiveCustomers AS
SELECT * FROM Customers WHERE IsActive = 1;
```

- Query is executed every time the view is accessed.

---

### Example – Materialized View (PostgreSQL)

```sql
CREATE MATERIALIZED VIEW SalesSummary AS
SELECT Region, SUM(Total) AS TotalSales
FROM Orders
GROUP BY Region;
```

```sql
-- Refresh to update the data
REFRESH MATERIALIZED VIEW SalesSummary;
```

---

### Use Cases

- Expensive joins and aggregations
- Dashboards and reports
- Data warehousing

---

## 18. **How do you handle NULL values in queries and constraints?**

**Answer:**  
`NULL` represents **unknown or missing values** in SQL. Handling them properly is critical to maintain data accuracy and avoid logic bugs.

---

### Handling NULLs in Queries

- Use `IS NULL` or `IS NOT NULL` to check for NULLs (not `=` or `!=`).
- Use functions like:
  - `COALESCE(x, fallback)` – returns first non-null value.
  - `ISNULL(x, fallback)` (SQL Server).
  - `IFNULL(x, fallback)` (MySQL).

```sql
-- Replace NULL with 'N/A'
SELECT COALESCE(LastName, 'N/A') AS DisplayName FROM Employees;
```

---

### Example – Filtering NULLs

```sql
SELECT * FROM Orders WHERE ShippedDate IS NULL;
```

---

### NULLs in Constraints

- **Primary keys**: Cannot contain NULLs.
- **Foreign keys**: Can reference NULL if allowed.
- **Unique constraints**: May allow NULLs depending on the RDBMS.

---

### Caution with NULLs in Comparisons

```sql
-- This will not return rows where Discount is NULL
SELECT * FROM Products WHERE Discount <> 0;
```

To include NULLs explicitly:

```sql
SELECT * FROM Products WHERE Discount <> 0 OR Discount IS NULL;
```

---

### Best Practices

- Always check for NULLs explicitly.
- Avoid assuming NULL = 0 or NULL = ''.
- Use default values where possible to prevent NULLs from entering the system.

---

## 19. **What is the difference between scalar functions and table-valued functions?**

**Answer:**  
SQL Server (and other RDBMSs) supports two main types of user-defined functions:

---

### Scalar Functions

- Return a **single value** (e.g., `INT`, `VARCHAR`, `DATE`, etc.)
- Used in SELECT, WHERE, and other clauses
- Accepts parameters and returns a single result

```sql
CREATE FUNCTION GetYearOnly (@date DATE)
RETURNS INT
AS
BEGIN
    RETURN YEAR(@date);
END;
```

```sql
SELECT dbo.GetYearOnly('2024-05-15'); -- Returns: 2024
```

---

### Table-Valued Functions (TVF)

- Return a **table** (set of rows)
- Can be used like a regular table in `FROM` clause
- Useful for encapsulating reusable queries

```sql
CREATE FUNCTION GetHighValueOrders (@minTotal DECIMAL(10,2))
RETURNS TABLE
AS
RETURN (
    SELECT * FROM Orders WHERE TotalAmount > @minTotal
);
```

```sql
SELECT * FROM dbo.GetHighValueOrders(1000);
```

---

### Comparison

| Feature              | Scalar Function          | Table-Valued Function      |
|----------------------|--------------------------|----------------------------|
| Return Type          | Single value             | Table (multiple rows)      |
| Use Case             | Inline calculations      | Reusable query logic       |
| Use in Query         | SELECT/WHERE             | FROM clause                |

---

## 10. **How would you design a schema for a multi-tenant application in SQL?**

**Answer:**  
Multi-tenant applications serve multiple customers (**tenants**) using a shared or isolated database schema. There are 3 common approaches:

---

### 1. **Single Database, Shared Schema**

- All tenants share the same tables.
- Tenant-specific data is separated by a `TenantId` column.

```sql
CREATE TABLE Orders (
    Id INT PRIMARY KEY,
    TenantId INT,
    CustomerId INT,
    Total DECIMAL(10,2)
);
```

- ✅ Easy to scale
- ❌ Requires strict row-level security and indexing

---

### 2. **Single Database, Separate Schema per Tenant**

- Each tenant has their **own schema** within one database.
- E.g., `Tenant1.Orders`, `Tenant2.Orders`

- ✅ Easier logical separation
- ❌ Schema deployment and maintenance become more complex

---

### 3. **Database per Tenant**

- Each tenant gets a **separate physical database**.
- Most secure and isolated.

- ✅ Maximum isolation and customization
- ❌ Expensive to manage and scale

---

### Best Practices

- Always enforce data isolation via `TenantId` filtering
- Use policies, views, or stored procedures to abstract tenant access
- Automate schema updates across tenants
- Use indexed `TenantId` to maintain query performance

---

### Bonus: Add a tenant filter using a view

```sql
CREATE VIEW CurrentTenantOrders AS
SELECT * FROM Orders
WHERE TenantId = CURRENT_TENANT_ID();
```

*(Assumes `CURRENT_TENANT_ID()` is resolved by your app context or session.)*

---

