# Experiment 2: DDL Commands

## AIM
To study and implement DDL commands and different types of constraints.

## THEORY

### 1. CREATE
Used to create a new relation (table).

**Syntax:**
```sql
CREATE TABLE (
  field_1 data_type(size),
  field_2 data_type(size),
  ...
);
```
### 2. ALTER
Used to add, modify, drop, or rename fields in an existing relation.
(a) ADD
```sql
ALTER TABLE std ADD (Address CHAR(10));
```
(b) MODIFY
```sql
ALTER TABLE relation_name MODIFY (field_1 new_data_type(size));
```
(c) DROP
```sql
ALTER TABLE relation_name DROP COLUMN field_name;
```
(d) RENAME
```sql
ALTER TABLE relation_name RENAME COLUMN old_field_name TO new_field_name;
```
### 3. DROP TABLE
Used to permanently delete the structure and data of a table.
```sql
DROP TABLE relation_name;
```
### 4. RENAME
Used to rename an existing database object.
```sql
RENAME TABLE old_relation_name TO new_relation_name;
```
### CONSTRAINTS
Constraints are used to specify rules for the data in a table. If there is any violation between the constraint and the data action, the action is aborted by the constraint. It can be specified when the table is created (using CREATE TABLE) or after it is created (using ALTER TABLE).
### 1. NOT NULL
When a column is defined as NOT NULL, it becomes mandatory to enter a value in that column.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) NOT NULL
);
```
### 2. UNIQUE
Ensures that values in a column are unique.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) UNIQUE
);
```
### 3. CHECK
Specifies a condition that each row must satisfy.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) CHECK (logical_expression)
);
```
### 4. PRIMARY KEY
Used to uniquely identify each record in a table.
Properties:
Must contain unique values.
Cannot be null.
Should contain minimal fields.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) PRIMARY KEY
);
```
### 5. FOREIGN KEY
Used to reference the primary key of another table.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size),
  FOREIGN KEY (column_name) REFERENCES other_table(column)
);
```
### 6. DEFAULT
Used to insert a default value into a column if no value is specified.

Syntax:
```sql
CREATE TABLE Table_Name (
  col_name1 data_type,
  col_name2 data_type,
  col_name3 data_type DEFAULT 'default_value'
);
```

**Question 1**
---
```
Insert all students from Archived_students table into the Student_details table.

cid         name        type        notnull     dflt_value  pk
----------  ----------  ----------  ----------  ----------  ----------
0           RollNo      INT           0                       1
1           Name        VARCHAR(100)  0                       0
2           Gender      VARCHAR(10)   0                       0
3           Subject     VARCHAR(50)   0                       0
4           MARKS       INT           0                       0
For example:

Test	Result
select * from student_details;
RollNo      Name           Gender      Subject     MARKS
----------  -------------  ----------  ----------  ----------
1           Alice Johnson  Female      Math        85
2           Bob Smith      Male        Science     90
3           Charlie Brown  Male        English     78

```
**sql**
```
INSERT INTO Student_details
SELECT * FROM Archived_students;
```

**Output:**

<img width="1061" height="333" alt="image" src="https://github.com/user-attachments/assets/cab24095-eaa0-4601-827b-c8795eb16eb7" />


**Question 2**
---
```
Write a SQL query to Rename the "city" column to "location" in the "customer" table.

Sample table: customer

 customer_id |   cust_name    |    city    | grade | salesman_id 
-------------+----------------+------------+-------+-------------
        3002 | Nick Rimando   | New York   |   100 |        5001
        3007 | Brad Davis     | New York   |   200 |        5001
        3005 | Graham Zusi    | California |   200 |        5002
 

For example:

Test	Result
pragma table_info('customer');
cid         name         type                               notnull     dflt_value  pk
----------  -----------  ---------------------------------  ----------  ----------  ----------
0           customer_id  integer primarykey auto increment  0                       0
1           cust_name    varchar2(30)                       0                       0
2           location     varchar(30)                        0                       0
3           grade        number                             0                       0
4           salesman_id  number                             0                       0
```
**sql**

```
ALTER TABLE customer
RENAME COLUMN city TO location;
```

**Output:**
<img width="1065" height="367" alt="image" src="https://github.com/user-attachments/assets/59259832-bed1-4c43-9bf1-6847e8d7debe" />


**Question 3**
---
```
Create a table named Bonuses with the following constraints:
BonusID as INTEGER should be the primary key.
EmployeeID as INTEGER should be a foreign key referencing Employees(EmployeeID).
BonusAmount as REAL should be greater than 0.
BonusDate as DATE.
Reason as TEXT should not be NULL.
For example:

Test	Result
INSERT INTO Bonuses (BonusID, EmployeeID, BonusAmount, BonusDate, Reason) VALUES (1, 6, 1000.0, '2024-08-01', 'Outstanding performance');
SELECT * FROM Bonuses;
BonusID     EmployeeID  BonusAmount  BonusDate   Reason
----------  ----------  -----------  ----------  -----------------------
1           6           1000.0       2024-08-01  Outstanding performance
```
**sql**

```
CREATE TABLE Bonuses (
    BonusID INTEGER PRIMARY KEY,
    EmployeeID INTEGER,
    BonusAmount REAL CHECK (BonusAmount > 0),
    BonusDate DATE,
    Reason TEXT NOT NULL,
    FOREIGN KEY (EmployeeID) REFERENCES Employees(EmployeeID)
);
```
**Output:**

<img width="1068" height="332" alt="image" src="https://github.com/user-attachments/assets/ef9d58ed-5486-4c67-9a5f-b26625c837fb" />


**Question 4**
---
```
Create a table named Department with the following constraints:
DepartmentID as INTEGER should be the primary key.
DepartmentName as TEXT should be unique and not NULL.
Location as TEXT.
For example:

Test	Result
INSERT INTO Department (DepartmentID, DepartmentName, Location) VALUES (1, 'Human Resources', 'New York');
select * from Department;
DepartmentID  DepartmentName   Location
------------  ---------------  ----------
1             Human Resources  New York
```
**sql**
```
 CREATE TABLE Department (
    DepartmentID INTEGER PRIMARY KEY,
    DepartmentName TEXT UNIQUE NOT NULL,
    Location TEXT
);
```

**Output:**

<img width="1304" height="376" alt="image" src="https://github.com/user-attachments/assets/769b4bdf-aea9-4c5a-b744-8b740bb028f7" />


**Question 5**
---
```
-- Create a table named Orders with the following constraints:
OrderID as INTEGER should be the primary key.
OrderDate as DATE should be not NULL.
CustomerID as INTEGER should be a foreign key referencing Customers(CustomerID).
For example:

Test	Result
INSERT INTO Customers (CustomerID, FirstName, LastName, Email) VALUES (1, 'Alice', 'Johnson', 'alice.johnson@example.com');
INSERT INTO Orders (OrderID, OrderDate, CustomerID) VALUES (1, '2024-08-01', 1);
select * from orders;
OrderID     OrderDate   CustomerID
----------  ----------  ----------
1           2024-08-01  1

```
**sql**
```
-- CREATE TABLE Orders (
    OrderID INTEGER PRIMARY KEY,
    OrderDate DATE NOT NULL,
    CustomerID INTEGER,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);
```

**Output:**

<img width="1290" height="392" alt="image" src="https://github.com/user-attachments/assets/577e0295-de6a-4cda-af1a-632e7247be64" />


**Question 6**
---
```
--Insert the following products into the Products table:

Name        Category     Price       Stock
----------  -----------  ----------  ----------
Smartphone  Electronics  800         150
Headphones  Accessories  200         300
For example:

Test	Result
SELECT Name, Category, Price, Stock FROM Products;

Name        Category     Price       Stock
----------  -----------  ----------  ----------
Smartphone  Electronics  800         150
Headphones  Accessories  200         300


```
**sql**
```
 INSERT INTO Products (Name, Category, Price, Stock)
VALUES
('Smartphone', 'Electronics', 800, 150),
('Headphones', 'Accessories', 200, 300);
```

**Output:**

<img width="1279" height="451" alt="image" src="https://github.com/user-attachments/assets/c06dcd4b-89cb-4f82-ba29-222de0608439" />

**Question 7**
---
```
 Create a table named Reviews with the following columns:

ReviewID as INTEGER
ProductID as INTEGER
Rating as REAL
ReviewText as TEXT
For example:

Test	Result
pragma table_info('Reviews');
cid         name        type        notnull     dflt_value  pk
----------  ----------  ----------  ----------  ----------  ----------
0           ReviewID    INTEGER     0                       0
1           ProductID   INTEGER     0                       0
2           Rating      REAL        0                       0
3           ReviewText  TEXT        0                       0


```
**sql**
```
CREATE TABLE Reviews (
    ReviewID INTEGER,
    ProductID INTEGER,
    Rating REAL,
    ReviewText TEXT
);
```

**Output:**

<img width="1298" height="512" alt="image" src="https://github.com/user-attachments/assets/6f510d05-0876-4249-bf05-653f96f757e0" />


**Question 8**
---
```
-- Write an SQL query to change the name of the column id to employee_id in the table employee.

For example:

Test	Result
pragma table_info('employee');
cid         name         type        notnull     dflt_value  pk
----------  -----------  ----------  ----------  ----------  ----------
0           employee_id  integer     0                       0
1           salary       number      0                       0

```
**sql**
```
 ALTER TABLE employee
RENAME COLUMN id TO employee_id;
```

**Output:**
<img width="1287" height="378" alt="image" src="https://github.com/user-attachments/assets/67b695b8-878a-48ae-bf35-5a0b22edac7d" />


**Question 9**
---
```
-- Insert a record with EmployeeID 001, Name Sarah Parker, Position Manager, Department HR, and Salary 60000 into the Employee table.

For example:

Test	Result
SELECT * FROM Employee WHERE EmployeeID = 001;
EmployeeID  Name          Position    Department  Salary
----------  ------------  ----------  ----------  ----------
1           Sarah Parker  Manager     HR          60000


```
**sql**
```
INSERT INTO Employee (EmployeeID, Name, Position, Department, Salary)
VALUES (1, 'Sarah Parker', 'Manager', 'HR', 60000);
```

**Output:**
<img width="1274" height="339" alt="image" src="https://github.com/user-attachments/assets/6819a432-17fb-4301-8854-73151cd2c61a" />


**Question 10**
---
```
Create a table named Shipments with the following constraints:
ShipmentID as INTEGER should be the primary key.
ShipmentDate as DATE.
SupplierID as INTEGER should be a foreign key referencing Suppliers(SupplierID).
OrderID as INTEGER should be a foreign key referencing Orders(OrderID).
For example:

Test	Result
INSERT INTO Shipments (ShipmentID, ShipmentDate, SupplierID, OrderID) VALUES (2, '2024-08-03', 99, 1);
Error: FOREIGN KEY constraint failed

```
**sql**
```
 CREATE TABLE Shipments (
    ShipmentID INTEGER PRIMARY KEY,
    ShipmentDate DATE,
    SupplierID INTEGER,
    OrderID INTEGER,
    FOREIGN KEY (SupplierID) REFERENCES Suppliers(SupplierID),
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID)
);
```

**Output:**

<img width="1262" height="344" alt="image" src="https://github.com/user-attachments/assets/4f1b6b09-3663-43a9-a092-a8830ef6b931" />


## RESULT
Thus, the SQL queries to implement different types of constraints and DDL commands have been executed successfully.
