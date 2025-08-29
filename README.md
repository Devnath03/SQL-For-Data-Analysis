# SQL For Data Analysis

A concise, well-formatted reference for common SQL concepts, commands, and examples.

---

## Table of Contents
- Overview
- SQL Command Categories
- CREATE DATABASE & CREATE TABLE
- INSERT INTO examples
- ALTER TABLE examples
- SELECT / CRUD examples
- Quick Reference Cheat Sheet
- ER Diagram (script)

---

## Overview
SQL (Structured Query Language) is the standard language for working with relational databases. Use it to create structures (DDL), manipulate data (DML), query data (DQL), control transactions (TCL), and manage permissions (DCL).

---

## SQL Command Categories
| Category | Description |
|---:|---|
| DDL (Data Definition Language) | Create/modify/delete database objects (CREATE, DROP, ALTER, TRUNCATE) |
| DML (Data Manipulation Language) | Insert/update/delete data (INSERT, UPDATE, DELETE) |
| DQL (Data Query Language) | Query data (SELECT) |
| TCL (Transaction Control Language) | Transaction control (COMMIT, ROLLBACK, SAVEPOINT) |
| DCL (Data Control Language) | Permission control (GRANT, REVOKE) |

---

## Common Commands (Examples)

### DDL
```sql
-- Create a database
CREATE DATABASE SchoolDB;

-- Create a table
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Age INT,
    Grade CHAR(1)
);
```

### DML
```sql
-- Insert rows
INSERT INTO Students (StudentID, Name, Age, Grade)
VALUES (1, 'Alice', 20, 'A');

-- Update rows
UPDATE Students SET Age = 21 WHERE StudentID = 1;

-- Delete rows
DELETE FROM Students WHERE StudentID = 1;
```

### TCL
```sql
BEGIN;
-- some DML...
SAVEPOINT sp1;
-- more DML...
ROLLBACK TO sp1;
COMMIT;
```

### DCL
```sql
GRANT SELECT, INSERT ON Students TO user1;
REVOKE INSERT ON Students FROM user1;
```

---

## CREATE TABLE — Examples

Simple table:
```sql
CREATE TABLE Courses (
    CourseID INT PRIMARY KEY,
    CourseName VARCHAR(50) NOT NULL UNIQUE,
    Credits INT CHECK (Credits > 0),
    Department VARCHAR(30) DEFAULT 'General'
);
```

Table with foreign keys:
```sql
CREATE TABLE Enrollments (
    EnrollmentID INT PRIMARY KEY,
    StudentID INT,
    CourseID INT,
    EnrollmentDate DATE,
    FOREIGN KEY (StudentID) REFERENCES Students(StudentID),
    FOREIGN KEY (CourseID) REFERENCES Courses(CourseID)
);
```

---

## INSERT INTO — Examples

Insert into all columns:
```sql
INSERT INTO Students VALUES (1, 'Alice', 20, 'A');
```

Insert into specific columns:
```sql
INSERT INTO Students (StudentID, Name) VALUES (2, 'Bob');
```

Insert multiple rows:
```sql
INSERT INTO Students (StudentID, Name, Age, Grade) VALUES
  (3, 'Charlie', 22, 'B'),
  (4, 'Diana', 21, 'A');
```

Insert from another table:
```sql
INSERT INTO Alumni (StudentID, Name)
SELECT StudentID, Name FROM Students WHERE Grade = 'A';
```

---

## ALTER TABLE — Examples
```sql
-- Add a column
ALTER TABLE Students ADD Email VARCHAR(100);

-- Modify column (MySQL)
ALTER TABLE Students MODIFY Age BIGINT;

-- Drop column
ALTER TABLE Students DROP COLUMN Grade;

-- Rename column (dialect may vary)
ALTER TABLE Students RENAME COLUMN Name TO FullName;

-- Rename table (dialect may vary)
ALTER TABLE Students RENAME TO Learners;
```

---

## SELECT / CRUD Examples

Sample Employee table:
```sql
CREATE TABLE Employee (
    EmpID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Department VARCHAR(50),
    Age INT,
    PhoneNo VARCHAR(10)
);
```

Basic SELECT:
```sql
SELECT FirstName, LastName FROM Employee;
SELECT * FROM Employee;
```

WHERE / ORDER BY / DISTINCT / GROUP BY / HAVING / JOIN:
```sql
SELECT FirstName, Age FROM Employee WHERE Age = 25;
SELECT FirstName, Age FROM Employee ORDER BY Age DESC;
SELECT DISTINCT Department FROM Employee;
SELECT Department, COUNT(*) AS EmployeeCount FROM Employee GROUP BY Department;
SELECT Department, COUNT(*) FROM Employee GROUP BY Department HAVING COUNT(*) >= 2;
SELECT e.FirstName, p.Budget
FROM Employee e
JOIN Project p ON e.EmpID = p.EmpID;
```

UPDATE / DELETE:
```sql
UPDATE Staff SET PhoneNo = '9999999999' WHERE FirstName = 'Amelia';
DELETE FROM Staff WHERE Department = 'Marketing';
```

DROP / TRUNCATE:
```sql
DROP TABLE Staff;
TRUNCATE TABLE Staff;
```

---

## Quick Reference Cheat Sheet

| Statement | Purpose |
|---|---|
| SELECT | Retrieve rows |
| INSERT | Add rows |
| UPDATE | Modify rows |
| DELETE | Remove rows |
| CREATE TABLE | Define a table |
| ALTER TABLE | Change table structure |
| DROP TABLE | Remove table |
| TRUNCATE TABLE | Remove all rows (faster) |
| JOIN | Combine tables on keys |
| GROUP BY / HAVING | Aggregate and filter groups |
| ORDER BY | Sort results |

---

## ER Diagram Script (Python)
Use this script to generate a simple ER visualization (requires networkx & matplotlib).
```python
# python
import matplotlib.pyplot as plt
import networkx as nx

tables = {
    "Students": ["StudentID (PK)", "Name", "Age", "Grade"],
    "Courses": ["CourseID (PK)", "CourseName", "Credits", "Department"],
    "Enrollments": ["EnrollmentID (PK)", "StudentID (FK)", "CourseID (FK)", "EnrollmentDate"]
}

relationships = [
    ("Enrollments", "Students"),
    ("Enrollments", "Courses")
]

G = nx.DiGraph()
for table, columns in tables.items():
    label = f"{table}\n" + "\n".join(columns)
    G.add_node(table, label=label)
G.add_edges_from(relationships)

pos = nx.spring_layout(G, seed=42)
plt.figure(figsize=(10,6))
nx.draw_networkx_nodes(G, pos, node_size=3500, node_color="lightblue", edgecolors="black")
nx.draw_networkx_edges(G, pos, arrowstyle="->", arrowsize=20)
labels = nx.get_node_attributes(G, "label")
nx.draw_networkx_labels(G, pos, labels, font_size=9, font_family="monospace")

plt.title("ER Diagram: SchoolDB", fontsize=14)
plt.axis("off")
plt.show()
```

---

If you want emojis, tags, or a different tone (e.g., more examples per topic), specify which sections to expand.
FROM source_table
WHERE condition;

🔹 Parameters:

target_table → The table where data will be inserted.

source_table → The table from which data is selected.

🔹 Example:
-- Create another table
CREATE TABLE Alumni (
    StudentID INT,
    Name VARCHAR(50)
);

-- Insert data from Students table
INSERT INTO Alumni (StudentID, Name)
SELECT StudentID, Name
FROM Students
WHERE Grade = 'A';

🔹 Output (Alumni table):
StudentID	Name
1	Alice
4	Diana


📘 SQL ALTER TABLE Statement

The ALTER TABLE statement is used to change the structure of an existing table without deleting it.
Common operations include adding, modifying, deleting, or renaming columns, and even renaming the table itself.

1️⃣ ALTER TABLE ... ADD (Add New Column)
✅ Syntax:
ALTER TABLE table_name
ADD column_name datatype constraint;

🔹 Example:
-- Add Email column to Students table
ALTER TABLE Students
ADD Email VARCHAR(100);

🔹 Output (Students table structure):
Column	Type
StudentID	INT (PK)
Name	VARCHAR
Age	INT
Grade	CHAR(1)
Email	VARCHAR(100)
2️⃣ ALTER TABLE ... MODIFY (Modify Column Type/Size)

⚠️ In some databases (like MySQL), the keyword is MODIFY.
In SQL Server/Oracle, ALTER COLUMN is used instead.

✅ Syntax:
-- MySQL
ALTER TABLE table_name
MODIFY column_name new_datatype;

-- SQL Server/Oracle
ALTER TABLE table_name
ALTER COLUMN column_name new_datatype;

🔹 Example:
-- Change Age column to BIGINT
ALTER TABLE Students
MODIFY Age BIGINT;

🔹 Output:

Column Age is now BIGINT instead of INT.

3️⃣ ALTER TABLE ... DROP COLUMN (Delete a Column)
✅ Syntax:
ALTER TABLE table_name
DROP COLUMN column_name;

🔹 Example:
-- Remove Grade column
ALTER TABLE Students
DROP COLUMN Grade;

🔹 Output (Students table structure after drop):
Column	Type
StudentID	INT (PK)
Name	VARCHAR
Age	BIGINT
Email	VARCHAR(100)
4️⃣ ALTER TABLE ... RENAME COLUMN (Rename a Column)
✅ Syntax:
ALTER TABLE table_name
RENAME COLUMN old_column_name TO new_column_name;

🔹 Example:
-- Rename "Name" column to "FullName"
ALTER TABLE Students
RENAME COLUMN Name TO FullName;

🔹 Output (Students table structure):
Column	Type
StudentID	INT (PK)
FullName	VARCHAR
Age	BIGINT
Email	VARCHAR(100)
5️⃣ ALTER TABLE ... RENAME TO (Rename the Table)
✅ Syntax:
ALTER TABLE old_table_name
RENAME TO new_table_name;

🔹 Example:
-- Rename Students table to Learners
ALTER TABLE Students
RENAME TO Learners;

🔹 Output:

The table Students is now called Learners.

🔎 Example Queries (All Together)
-- 1. Add a column
ALTER TABLE Students ADD Email VARCHAR(100);

-- 2. Modify column data type
ALTER TABLE Students MODIFY Age BIGINT;

-- 3. Drop a column
ALTER TABLE Students DROP COLUMN Grade;

-- 4. Rename a column
ALTER TABLE Students RENAME COLUMN Name TO FullName;

-- 5. Rename the table
ALTER TABLE Students RENAME TO Learners;


# SQL SELECT Statement

The SELECT statement is used to retrieve data from one or more tables. It can target specific columns or all columns, and supports filtering, sorting, grouping, joins, and aggregations.

Syntax
SELECT field1, field2, ...
FROM entity_name;


Parameters:

field1, field2: Columns you want to retrieve. Use * to select all columns.

entity_name: Table or view you are querying.

Sample Table and Data

We'll create a table Employee with some sample data:

CREATE TABLE Employee (
    EmpID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Department VARCHAR(50),
    Age INT,
    ContactNo VARCHAR(10)
);

INSERT INTO Employee (EmpID, FirstName, LastName, Department, Age, ContactNo)
VALUES 
    (101, 'Ethan', 'Brown', 'Finance', 28, '9876543210'),
    (102, 'Olivia', 'Wilson', 'HR', 25, '9876543211'),
    (103, 'Hiro', 'Yamamoto', 'IT', 30, '9876543212'),
    (104, 'Miguel', 'Garcia', 'Finance', 25, '9876543213'),
    (105, 'Sofia', 'Bianchi', 'Marketing', 27, '9876543214');

1️⃣ Select Specific Columns

Retrieve only FirstName and LastName:

SELECT FirstName, LastName
FROM Employee;


Output:

FirstName	LastName
Ethan	Brown
Olivia	Wilson
Hiro	Yamamoto
Miguel	Garcia
Sofia	Bianchi
2️⃣ Select All Columns

Fetch all columns:

SELECT *
FROM Employee;


Output:

EmpID	FirstName	LastName	Department	Age	ContactNo
101	Ethan	Brown	Finance	28	9876543210
102	Olivia	Wilson	HR	25	9876543211
103	Hiro	Yamamoto	IT	30	9876543212
104	Miguel	Garcia	Finance	25	9876543213
105	Sofia	Bianchi	Marketing	27	9876543214
3️⃣ SELECT with WHERE Clause

Filter employees aged 25:

SELECT FirstName, Age
FROM Employee
WHERE Age = 25;


Output:

FirstName	Age
Olivia	25
Miguel	25
4️⃣ SELECT with ORDER BY Clause

Sort by Age descending:

SELECT FirstName, Age
FROM Employee
ORDER BY Age DESC;


Output:

FirstName	Age
Hiro	30
Ethan	28
Sofia	27
Olivia	25
Miguel	25
5️⃣ SELECT with DISTINCT Clause

Get unique departments:

SELECT DISTINCT Department
FROM Employee;


Output:

Department
Finance
HR
IT
Marketing
6️⃣ SELECT with GROUP BY Clause

Count employees in each department:

SELECT Department, COUNT(*) AS EmployeeCount
FROM Employee
GROUP BY Department;


Output:

Department	EmployeeCount
Finance	2
HR	1
IT	1
Marketing	1
7️⃣ SELECT with HAVING Clause

Departments with 2 or more employees:

SELECT Department, COUNT(*) AS EmployeeCount
FROM Employee
GROUP BY Department
HAVING COUNT(*) >= 2;


Output:

Department	EmployeeCount
Finance	2
8️⃣ SELECT with JOIN Clause

Create another table Project:

CREATE TABLE Project (
    ProjectID INT PRIMARY KEY,
    EmpID INT,
    Budget DECIMAL(10,2),
    FOREIGN KEY (EmpID) REFERENCES Employee(EmpID)
);

INSERT INTO Project (ProjectID, EmpID, Budget)
VALUES
(201, 101, 50000.00),
(202, 102, 30000.00),
(203, 103, 70000.00);


Join Employee and Project to get employee names and project budgets:

SELECT Employee.FirstName, Project.Budget
FROM Employee
JOIN Project
ON Employee.EmpID = Project.EmpID;


Output:

FirstName	Budget
Ethan	50000.00
Olivia	30000.00
Hiro	70000.00


1️⃣ WHERE Clause

The WHERE clause filters rows that satisfy a condition.

Syntax:

SELECT column1, column2
FROM table_name
WHERE condition;


Example: Get all staff in the Sales department:

SELECT FirstName, Department
FROM Staff
WHERE Department = 'Sales';


Output:

FirstName	Department
Lucas	Sales
Diego	Sales
2️⃣ UPDATE Statement

The UPDATE statement modifies existing data.

Syntax:

UPDATE table_name
SET column1 = value1, column2 = value2
WHERE condition;


Example: Update the PhoneNo of Amelia:

UPDATE Staff
SET PhoneNo = '9999999999'
WHERE FirstName = 'Amelia';


After Update:

StaffID	FirstName	PhoneNo
305	Amelia	9999999999
3️⃣ DELETE Statement

The DELETE statement removes rows from a table.

Syntax:

DELETE FROM table_name
WHERE condition;


Example: Delete staff in the Marketing department:

DELETE FROM Staff
WHERE Department = 'Marketing';


Remaining Staff:

StaffID	FirstName	Department
301	Lucas	Sales
303	Riku	IT
304	Diego	Sales
305	Amelia	Finance

⚠️ Important: Without WHERE, all rows in the table will be deleted.

4️⃣ ORDER BY Clause

The ORDER BY clause sorts query results in ascending (ASC) or descending (DESC) order.

Syntax:

SELECT column1, column2
FROM table_name
ORDER BY column1 ASC|DESC;


Example: Sort staff by Age descending:

SELECT FirstName, Age
FROM Staff
ORDER BY Age DESC;


Output:

FirstName	Age
Riku	31
Lucas	29
Amelia	28
Emma	27
Diego	27
5️⃣ DROP Table

The DROP statement removes a table completely, including its data and structure.

Syntax:

DROP TABLE table_name;


Example: Drop the Staff table:

DROP TABLE Staff;


After this, the Staff table no longer exists.

6️⃣ TRUNCATE Table

The TRUNCATE statement removes all rows from a table but keeps the table structure intact.

Syntax:

TRUNCATE TABLE table_name;


Example: Remove all staff data but keep the table:

TRUNCATE TABLE Staff;


Effect: Table Staff remains, but it has 0 rows.

✅ Quick Reference Table
Statement	Purpose	Key Notes
WHERE	Filter rows	Used with SELECT, UPDATE, DELETE
UPDATE	Modify existing rows	Always use WHERE to avoid updating all rows
DELETE	Remove rows	Always use WHERE to avoid deleting all rows
ORDER BY	Sort results	Use ASC or DESC
DROP	Remove table completely	Cannot be undone
TRUNCATE	Remove all data	Keeps table structure, faster than DELETE

SQL CRUD + Table Management Cheat Sheet
SQL Statement	Syntax	Example	Description	Output / Effect
SELECT	SELECT column1, column2 FROM table_name;	SELECT FirstName, LastName FROM Employee;	Retrieves specific columns	List of employees’ first and last names
SELECT ALL	SELECT * FROM table_name;	SELECT * FROM Employee;	Retrieves all columns	Full table data
WHERE	SELECT column FROM table_name WHERE condition;	SELECT FirstName FROM Employee WHERE Department='IT';	Filters rows based on a condition	Employees in IT department
ORDER BY	`SELECT column FROM table_name ORDER BY column ASC	DESC;`	SELECT FirstName, Age FROM Employee ORDER BY Age DESC;	Sorts query results
DISTINCT	SELECT DISTINCT column FROM table_name;	SELECT DISTINCT Department FROM Employee;	Returns unique values	List of unique departments
GROUP BY	SELECT column, COUNT(*) FROM table_name GROUP BY column;	SELECT Department, COUNT(*) AS DeptCount FROM Employee GROUP BY Department;	Aggregates rows	Number of employees per department
HAVING	SELECT column, COUNT(*) FROM table_name GROUP BY column HAVING COUNT(*) >= 2;	SELECT Department, COUNT(*) AS DeptCount FROM Employee GROUP BY Department HAVING COUNT(*) >= 2;	Filters grouped results	Departments with 2+ employees
JOIN	SELECT a.column1, b.column2 FROM table1 a JOIN table2 b ON a.key=b.key;	SELECT Employee.FirstName, Project.Budget FROM Employee JOIN Project ON Employee.EmpID = Project.EmpID;	Combines tables	Employee names with project budgets
UPDATE	UPDATE table_name SET column=value WHERE condition;	UPDATE Staff SET PhoneNo='9999999999' WHERE FirstName='Amelia';	Modifies existing rows	Updates specific staff’s phone number
DELETE	DELETE FROM table_name WHERE condition;	DELETE FROM Staff WHERE Department='Marketing';	Removes rows	Deletes specific rows
TRUNCATE	TRUNCATE TABLE table_name;	TRUNCATE TABLE Staff;	Removes all rows, keeps structure	Table becomes empty
DROP	DROP TABLE table_name;	DROP TABLE Staff;	Removes table completely	Table structure and data deleted
Sample Tables Used in Examples
Employee Table
CREATE TABLE Employee (
    EmpID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Department VARCHAR(50),
    Age INT,
    PhoneNo VARCHAR(10)
);

Project Table
CREATE TABLE Project (
    ProjectID INT PRIMARY KEY,
    EmpID INT,
    Budget DECIMAL(10,2),
    FOREIGN KEY (EmpID) REFERENCES Employee(EmpID)
);

