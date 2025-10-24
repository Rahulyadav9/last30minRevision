Sure! Here’s a **concise last-minute MySQL cheat sheet** with the **most commonly asked queries and concepts** for interviews or quick revision:

---

## **1️⃣ Basic Queries**

```sql
-- Select all records
SELECT * FROM EMPLOYEE;

-- Select specific columns
SELECT name, dept FROM EMPLOYEE;

-- Filter rows
SELECT * FROM EMPLOYEE WHERE dept = 'Sales';

-- Sort results
SELECT * FROM EMPLOYEE ORDER BY name ASC;

-- Limit rows
SELECT * FROM EMPLOYEE LIMIT 5;

-- Count rows
SELECT COUNT(*) FROM EMPLOYEE;
```

---

## **2️⃣ Aggregate Functions**

```sql
SELECT COUNT(*), AVG(salary), MIN(salary), MAX(salary), SUM(salary)
FROM EMPLOYEE;

-- Group by department
SELECT dept, COUNT(*) FROM EMPLOYEE GROUP BY dept;

-- Filter groups
SELECT dept, COUNT(*) FROM EMPLOYEE GROUP BY dept HAVING COUNT(*) > 2;
```

---

## **3️⃣ Joins**

```sql
-- INNER JOIN
SELECT e.name, d.dept_name
FROM EMPLOYEE e
INNER JOIN DEPARTMENT d ON e.dept = d.dept_id;

-- LEFT JOIN
SELECT e.name, d.dept_name
FROM EMPLOYEE e
LEFT JOIN DEPARTMENT d ON e.dept = d.dept_id;

-- RIGHT JOIN
SELECT e.name, d.dept_name
FROM EMPLOYEE e
RIGHT JOIN DEPARTMENT d ON e.dept = d.dept_id;
```

---

## **4️⃣ Subqueries**

```sql
-- Find employees with salary > average
SELECT * FROM EMPLOYEE
WHERE salary > (SELECT AVG(salary) FROM EMPLOYEE);

-- Using IN
SELECT * FROM EMPLOYEE
WHERE dept IN (SELECT dept_id FROM DEPARTMENT WHERE location = 'NY');
```

---

## **5️⃣ Stored Procedure**

```sql
DELIMITER //
CREATE PROCEDURE GetEmployeeByID(IN empID INT)
BEGIN
    SELECT * FROM EMPLOYEE WHERE empId = empID;
END //
DELIMITER ;

-- Call it
CALL GetEmployeeByID(4);
```

---

## **6️⃣ Update / Delete / Insert**

```sql
-- Insert
INSERT INTO EMPLOYEE(empId, name, dept) VALUES (7, 'Seema', 'Sales');

-- Update
UPDATE EMPLOYEE SET dept='Marketing' WHERE empId=5;

-- Delete
DELETE FROM EMPLOYEE WHERE empId=6;
```

---

## **7️⃣ Indexes**

```sql
-- Create index
CREATE INDEX idx_name ON EMPLOYEE(name);

-- Drop index
DROP INDEX idx_name ON EMPLOYEE;
```

---

## **8️⃣ Useful Functions**

```sql
-- Handle NULLs
SELECT IFNULL(salary, 0) FROM EMPLOYEE;

-- Concatenate strings
SELECT CONCAT(name, ' - ', dept) AS emp_info FROM EMPLOYEE;

-- Date functions
SELECT NOW();            -- Current date & time
SELECT CURDATE();        -- Current date
SELECT DATE_ADD(NOW(), INTERVAL 7 DAY);  -- Add 7 days
```

---

## **9️⃣ Transactions**

```sql
START TRANSACTION;

UPDATE ACCOUNT SET balance = balance - 100 WHERE id = 1;
UPDATE ACCOUNT SET balance = balance + 100 WHERE id = 2;

COMMIT;   -- Save changes
ROLLBACK; -- Undo changes
```

---

## **10️⃣ Last-Minute Tips**

* Know **JOIN types** and **differences between WHERE and HAVING**.
* Understand **primary key vs foreign key**.
* Practice **GROUP BY with aggregate functions**.
* Use `EXPLAIN` for **query optimization**.
* Be ready to write a **stored procedure** or **simple subquery**.

---



11️⃣ VIEWs in MySQL
What is a VIEW?

A VIEW is a virtual table created by a SELECT query.

It does not store data physically (except for materialized views, which MySQL doesn’t support natively).

It’s useful for simplifying queries, improving security, and reusing complex queries.
```sql
CREATE VIEW EmployeeView AS
SELECT empId, name, dept
FROM EMPLOYEE
WHERE dept = 'Sales';

CREATE VIEW EmployeeDeptView AS
SELECT e.empId, e.name, d.dept_name
FROM EMPLOYEE e
INNER JOIN DEPARTMENT d ON e.dept = d.dept_id;

-- Query the view
SELECT * FROM EmployeeDeptView;
```

