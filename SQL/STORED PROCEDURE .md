# 📘 STORED PROCEDURE BASICS (SQL Server)

## 🔹 What is a Stored Procedure?
A stored procedure is a saved SQL code (a block of queries) that you can run anytime with a single command.

- Think of it like a pre-written recipe you can reuse instead of writing the full code again and again.

### 🏗️ STEP 1: Creating a Table for Practice
**✅ Table: Employees**
```
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY IDENTITY(1,1),
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Department VARCHAR(50),
    Salary DECIMAL(10,2),
    HireDate DATE,
    City VARCHAR(50)
);
```
### ✅ Step 2: Insert Sample Data (100 Rows)
```
-- Sample Insert
INSERT INTO Employees (FirstName, LastName, Department, Salary, HireDate, City)
VALUES ('John', 'Doe', 'IT', 75000.00, '2020-01-15', 'New York');

-- Use this script to generate 100 rows:
DECLARE @i INT = 1;

WHILE @i <= 100
BEGIN
    INSERT INTO Employees (FirstName, LastName, Department, Salary, HireDate, City)
    VALUES (
        CONCAT('First', @i),
        CONCAT('Last', @i),
        CHOOSE((@i % 5) + 1, 'IT', 'HR', 'Finance', 'Marketing', 'Operations'),
        CAST(ROUND(RAND() * (100000 - 30000) + 30000, 2) AS DECIMAL(10,2)),
        DATEADD(DAY, -@i * 30, GETDATE()),
        CHOOSE((@i % 5) + 1, 'New York', 'Chicago', 'San Francisco', 'Miami', 'Seattle')
    );
    SET @i += 1;
END;
```
## 🧠 Types of Stored Procedures
**1️⃣ Without Parameters**
Runs a fixed task. No input needed.
```
CREATE PROCEDURE GetAllEmployees
AS
BEGIN
    SELECT * FROM Employees ORDER BY HireDate DESC;
END;
```
▶️ Usage:
`EXEC GetAllEmployees;`

**2️⃣ With One Parameter**
2️⃣ With One Parameter
Takes one input to filter or customize the result.
```
CREATE PROCEDURE GetEmployeesByDepartment
    @Dept VARCHAR(50)
AS
BEGIN
    SELECT * FROM Employees WHERE Department = @Dept;
END;
```
▶️ Usage:
`EXEC GetEmployeesByDepartment 'IT';`

**3️⃣ With Multiple Parameters**
Takes multiple inputs to filter the result better.
```
CREATE PROCEDURE GetEmployeesByDeptAndSalary
    @Dept VARCHAR(50),
    @MinSalary DECIMAL(10,2),
    @MaxSalary DECIMAL(10,2)
AS
BEGIN
    SELECT * 
    FROM Employees 
    WHERE Department = @Dept AND Salary BETWEEN @MinSalary AND @MaxSalary;
END;
```
▶️ Usage:
`EXEC GetEmployeesByDeptAndSalary 'Finance', 40000, 80000;`

## 🔍 How to View All Stored Procedures?
**1.🧾 See all procedures:**
`SELECT name FROM sys.procedures;`

**2.🔎 Filter by name:**
`SELECT name FROM sys.procedures WHERE name LIKE '%Employee%'`

## ✅  ALTER and DROP a Procedure

**Modify procedure logic:**
```
ALTER PROCEDURE GetAllEmployees 
AS 
BEGIN 
    SELECT * FROM Employees; 
END;
```

**Remove procedure:**
`DROP PROCEDURE GetAllEmployees;`



