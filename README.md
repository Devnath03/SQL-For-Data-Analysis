# SQL-For-Data-Analysis

Basis For SQL
SQL (Structured Query Language) is a standard programming language used to communicate with databases. It allows you to store, retrieve, update, and manage data efficiently.

📘 SQL Commands Explanation
1️⃣ DDL (Data Definition Language) – Structure of Database
Command	Description	Syntax Example
CREATE	Used to create a new database or table.	sql CREATE TABLE Students (ID INT PRIMARY KEY, Name VARCHAR(50), Age INT);
DROP	Deletes a table or database permanently.	sql DROP TABLE Students;
ALTER	Modifies an existing table (add, delete, or modify columns).	sql ALTER TABLE Students ADD Grade CHAR(1);
TRUNCATE	Removes all rows from a table but keeps its structure.	sql TRUNCATE TABLE Students;
2️⃣ DML (Data Manipulation Language) – Manage Data
Command	Description	Syntax Example
INSERT	Adds new data into a table.	sql INSERT INTO Students (ID, Name, Age) VALUES (1, 'Alice', 20);
UPDATE	Modifies existing data in a table.	sql UPDATE Students SET Age = 21 WHERE Name = 'Alice';
DELETE	Removes specific records from a table.	sql DELETE FROM Students WHERE ID = 1;
3️⃣ TCL (Transaction Control Language) – Manage Transactions
Command	Description	Syntax Example
COMMIT	Saves all changes made during the transaction.	sql COMMIT;
SAVEPOINT	Creates a savepoint within a transaction.	sql SAVEPOINT sp1;
ROLLBACK	Undo changes up to the last SAVEPOINT or entire transaction.	sql ROLLBACK TO sp1;
4️⃣ DQL (Data Query Language) – Query Data
Command	Description	Syntax Example
SELECT	Retrieves data from a table.	sql SELECT Name, Age FROM Students WHERE Age > 18;
5️⃣ DCL (Data Control Language) – Control Access
Command	Description	Syntax Example
GRANT	Gives user access privileges.	sql GRANT SELECT, INSERT ON Students TO user1;
REVOKE	Removes user access privileges.	sql REVOKE INSERT ON Students FROM user1;

📘 CREATE DATABASE & CREATE TABLE in SQL
1️⃣ CREATE DATABASE

Purpose: Used to create a new database.

Notes: Database name should be unique.

Syntax:

CREATE DATABASE database_name;


Example:

CREATE DATABASE SchoolDB;


👉 This will create a new database named SchoolDB.

2️⃣ CREATE TABLE

Purpose: Used to create a new table inside a database.

Notes:

Each column must have a name and data type.

One or more columns can be defined as PRIMARY KEY.

Constraints like NOT NULL, UNIQUE, DEFAULT, CHECK, etc., can be added.

Syntax:

CREATE TABLE table_name (
    column1 datatype constraint,
    column2 datatype constraint,
    ...
);

🔹 Example 1: Creating a Simple Table
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Age INT,
    Grade CHAR(1)
);


👉 This creates a Students table with 4 columns:

StudentID → Integer, Primary Key (unique ID).

Name → String, cannot be NULL.

Age → Integer.

Grade → Single character (e.g., 'A', 'B').

🔹 Example 2: Table with Constraints
CREATE TABLE Courses (
    CourseID INT PRIMARY KEY,
    CourseName VARCHAR(50) NOT NULL UNIQUE,
    Credits INT CHECK (Credits > 0),
    Department VARCHAR(30) DEFAULT 'General'
);


👉 Features:

CourseID → Primary Key.

CourseName → Must be unique and not NULL.

Credits → Must be greater than 0 (CHECK).

Department → If not provided, defaults to 'General'.

🔹 Example 3: Table with Foreign Key
CREATE TABLE Enrollments (
    EnrollmentID INT PRIMARY KEY,
    StudentID INT,
    CourseID INT,
    EnrollmentDate DATE,
    FOREIGN KEY (StudentID) REFERENCES Students(StudentID),
    FOREIGN KEY (CourseID) REFERENCES Courses(CourseID)
);


👉 This creates a relationship:

StudentID in Enrollments links to Students table.

CourseID in Enrollments links to Courses table.

⚡ Together:

-- Create Database
CREATE DATABASE SchoolDB;

-- Select Database (MySQL / SQL Server style)
USE SchoolDB;

-- Create Tables
CREATE TABLE Students (...);
CREATE TABLE Courses (...);
CREATE TABLE Enrollments (...);

# Diagram (ER model) for this SchoolDB so you can visualize the relationships
import matplotlib.pyplot as plt
import networkx as nx

# Define tables and relationships
tables = {
    "Students": ["StudentID (PK)", "Name", "Age", "Grade"],
    "Courses": ["CourseID (PK)", "CourseName", "Credits", "Department"],
    "Enrollments": ["EnrollmentID (PK)", "StudentID (FK)", "CourseID (FK)", "EnrollmentDate"]
}

# Define relationships (edges)
relationships = [
    ("Enrollments", "Students"),
    ("Enrollments", "Courses")
]

# Create graph
G = nx.DiGraph()

# Add nodes with attributes
for table, columns in tables.items():
    label = f"{table}\n" + "\n".join(columns)
    G.add_node(table, label=label)

# Add edges
G.add_edges_from(relationships)

# Layout
pos = nx.spring_layout(G, seed=42)

# Draw nodes with labels
plt.figure(figsize=(10,6))
nx.draw_networkx_nodes(G, pos, node_size=3500, node_color="lightblue", edgecolors="black")
nx.draw_networkx_edges(G, pos, arrowstyle="->", arrowsize=20)
labels = nx.get_node_attributes(G, "label")
nx.draw_networkx_labels(G, pos, labels, font_size=9, font_family="monospace")

plt.title("ER Diagram: SchoolDB", fontsize=14)
plt.axis("off")
plt.show()

📘 SQL INSERT INTO Statement

The INSERT INTO statement is used to add new rows of data into a table.

1️⃣ Inserting Data into All Columns
✅ Syntax:
INSERT INTO table_name
VALUES (value1, value2, value3, ...);

🔹 Parameters:

table_name → The target table where data is inserted.

VALUES → List of values to insert (must match the order of table columns).

🔹 Example:
-- Table
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    Name VARCHAR(50),
    Age INT,
    Grade CHAR(1)
);

-- Insert into all columns
INSERT INTO Students
VALUES (1, 'Alice', 20, 'A');

🔹 Output (Students table):
StudentID	Name	Age	Grade
1	Alice	20	A
2️⃣ Inserting Data into Specific Columns
✅ Syntax:
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);

🔹 Parameters:

column1, column2... → Only the listed columns get values.

Other columns will use default values or NULL if allowed.

🔹 Example:
INSERT INTO Students (StudentID, Name)
VALUES (2, 'Bob');

🔹 Output (Students table):
StudentID	Name	Age	Grade
1	Alice	20	A
2	Bob	NULL	NULL
3️⃣ Inserting Multiple Rows at Once
✅ Syntax:
INSERT INTO table_name (column1, column2, ...)
VALUES 
    (value1a, value2a, ...),
    (value1b, value2b, ...),
    (value1c, value2c, ...);

🔹 Parameters:

Multiple sets of values can be inserted in a single statement.

🔹 Example:
INSERT INTO Students (StudentID, Name, Age, Grade)
VALUES 
    (3, 'Charlie', 22, 'B'),
    (4, 'Diana', 21, 'A');

🔹 Output (Students table):
StudentID	Name	Age	Grade
1	Alice	20	A
2	Bob	NULL	NULL
3	Charlie	22	B
4	Diana	21	A
4️⃣ Inserting Data from One Table into Another
✅ Syntax:
INSERT INTO target_table (column1, column2, ...)
SELECT column1, column2, ...
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

