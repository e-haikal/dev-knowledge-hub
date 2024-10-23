# Basic SQL: Managing Employee Data

SQL (Structured Query Language) is essential for managing and querying relational databases. This guide provides a structured approach to creating a database, managing employee data, and performing various SQL operations. Each section includes clear examples, expected outputs, and best practices to make it easy for beginners to remember.

## Table of Contents
1. [Creating a Database](#creating-a-database)
2. [Using the Database](#using-the-database)
3. [Creating Tables](#creating-tables)
4. [Inserting Data](#inserting-data)
5. [Retrieving Data](#retrieving-data)
6. [Filtering Data](#filtering-data)
7. [Updating Data](#updating-data)
8. [Deleting Data](#deleting-data)
9. [Calculating Average Salary](#calculating-average-salary)
10. [Counting Employees by Department](#counting-employees-by-department)
11. [Best Practices](#best-practices)
12. [Conclusion](#conclusion)

---

## 1. Creating a Database

### Basic Structure
```sql
CREATE DATABASE database_name;
```

### Example
```sql
CREATE DATABASE CompanyDB;
```

### Expected Output
(No direct output, but the database "CompanyDB" is created.)

---

## 2. Using the Database

### Basic Structure
```sql
USE database_name;
```

### Example
```sql
USE CompanyDB;
```

### Expected Output
(No direct output, but the database is now in use.)

---

## 3. Creating Tables

### Basic Structure
```sql
CREATE TABLE table_name (
    column1 datatype constraints,
    column2 datatype constraints,
    ...
);
```

### Example
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY AUTO_INCREMENT,
    FirstName VARCHAR(50) NOT NULL,
    LastName VARCHAR(50) NOT NULL,
    Department VARCHAR(50) NOT NULL,
    Salary DECIMAL(10, 2) NOT NULL,
    HireDate DATE NOT NULL
);
```

### Expected Output
(No direct output, but the table "Employees" is created.)

---

## 4. Inserting Data

### Basic Structure
```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```

### Example
```sql
INSERT INTO Employees (FirstName, LastName, Department, Salary, HireDate)
VALUES 
('John', 'Doe', 'Engineering', 60000, '2020-01-15'),
('Jane', 'Smith', 'Marketing', 55000, '2019-03-20'),
('Sam', 'Brown', 'Sales', 50000, '2021-07-10'),
('Emily', 'Davis', 'Engineering', 62000, '2018-11-30'),
('Michael', 'Wilson', 'HR', 52000, '2020-06-12');
```

### Expected Output
(No direct output, but five rows of data are inserted into the "Employees" table.)

---

## 5. Retrieving Data

### Basic Structure
```sql
SELECT column1, column2, ...
FROM table_name;
```

### Example
```sql
SELECT * FROM Employees;
```

### Expected Output:

| EmployeeID | FirstName | LastName | Department   | Salary  | HireDate   |
|------------|-----------|----------|---------------|---------|------------|
| 1          | John      | Doe      | Engineering    | 60000.00| 2020-01-15 |
| 2          | Jane      | Smith    | Marketing      | 55000.00| 2019-03-20 |
| 3          | Sam       | Brown    | Sales          | 50000.00| 2021-07-10 |
| 4          | Emily     | Davis    | Engineering    | 62000.00| 2018-11-30 |
| 5          | Michael   | Wilson   | HR             | 52000.00| 2020-06-12 |

---

## 6. Filtering Data

### Basic Structure
```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

### Example
```sql
SELECT * FROM Employees WHERE Department = 'Engineering';
```

### Expected Output:

| EmployeeID | FirstName | LastName | Department   | Salary  | HireDate   |
|------------|-----------|----------|---------------|---------|------------|
| 1          | John      | Doe      | Engineering    | 60000.00| 2020-01-15 |
| 4          | Emily     | Davis    | Engineering    | 62000.00| 2018-11-30 |

---

## 7. Updating Data

### Basic Structure
```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

### Example
```sql
UPDATE Employees SET Salary = 65000 WHERE EmployeeID = 1;
```

### Expected Output (after retrieving data again):

```sql
SELECT * FROM Employees;
```

| EmployeeID | FirstName | LastName | Department   | Salary  | HireDate   |
|------------|-----------|----------|---------------|---------|------------|
| 1          | John      | Doe      | Engineering    | 65000.00| 2020-01-15 |
| 2          | Jane      | Smith    | Marketing      | 55000.00| 2019-03-20 |
| 3          | Sam       | Brown    | Sales          | 50000.00| 2021-07-10 |
| 4          | Emily     | Davis    | Engineering    | 62000.00| 2018-11-30 |
| 5          | Michael   | Wilson   | HR             | 52000.00| 2020-06-12 |

---

## 8. Deleting Data

### Basic Structure
```sql
DELETE FROM table_name
WHERE condition;
```

### Example
```sql
DELETE FROM Employees WHERE EmployeeID = 1;
```

### Expected Output (after retrieving data again):

```sql
SELECT * FROM Employees;
```

| EmployeeID | FirstName | LastName | Department   | Salary  | HireDate   |
|------------|-----------|----------|---------------|---------|------------|
| 2          | Jane      | Smith    | Marketing      | 55000.00| 2019-03-20 |
| 3          | Sam       | Brown    | Sales          | 50000.00| 2021-07-10 |
| 4          | Emily     | Davis    | Engineering    | 62000.00| 2018-11-30 |
| 5          | Michael   | Wilson   | HR             | 52000.00| 2020-06-12 |

---

## 9. Calculating Average Salary

### Basic Structure
```sql
SELECT AVG(column_name) AS alias_name
FROM table_name;
```

### Example
```sql
SELECT AVG(Salary) AS AverageSalary FROM Employees;
```

### Expected Output:

| AverageSalary |
|---------------|
| 58500.00      |

---

## 10. Counting Employees by Department

### Basic Structure
```sql
SELECT column_name, COUNT(*) AS alias_name
FROM table_name
GROUP BY column_name;
```

### Example
```sql
SELECT Department, COUNT(*) AS EmployeeCount FROM Employees GROUP BY Department;
```

### Expected Output:

| Department   | EmployeeCount |
|--------------|---------------|
| Engineering   | 2             |
| Marketing     | 1             |
| Sales         | 1             |
| HR            | 1             |

---

## 11. Best Practices

- **Use Clear Naming Conventions:** Use descriptive names for tables and columns.
- **Comment Your Code:** Add comments to clarify complex queries.
- **Use Transactions:** For multiple related operations, use transactions to maintain data integrity.
- **Regularly Backup Your Data:** Ensure you have backups to prevent data loss.

---

## 12. Conclusion

This guide has provided a structured approach to managing employee data with SQL, complete with example queries, expected outputs, and best practices. 

### Key Steps to Remember:
1. **Create a Database**
2. **Use the Database**
3. **Create Tables**
4. **Insert, Retrieve, Update, and Delete Data**
5. **Analyze Data with Aggregation Functions**

With this knowledge, you're ready to confidently work with SQL databases!

--- 
#### Reference
1. Explained by GPT