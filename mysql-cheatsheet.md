# Lisa's mySQL Cheat Sheet

<!-- ![alt text](lola.png) NEED A NEW PICTURE-->

### Brace yourself for a(nother) Syntax Guide themed on my loyal dog, Lola. :dog:

### 1. How to log into mysql from terminal
```
sudo mysql -u root -p
```

### 2. How to SHOW USERS 
```
SELECT User, Host FROM mysql.user;
```

### 3. How to CREATE USERS

```
CREATE USER 'username'@'hostname' IDENTIFIED BY 'password';

CREATE USER 'lolas_owner'@'localhost' IDENTIFIED BY 'lolas_password';

username = The username you want to create 
hostname = The host from which the user is allowed to connect. Use '%' for any host or 'localhost' for the local machine
password = The password associated with the Username created
```

### 4. How to GRANT PRIVILEGES, Show granted privileges and remove.
```
GRANT ALL PRIVILEGES ON database_name.* TO 'username'@'hostname';
FLUSH PRIVILEGES;

REVOKE ALL PRIVILEGES ON mydatabase.* FROM 'myuser'@'localhost';
FLUSH PRIVILEGES;
```

```
GRANT ALL PRIVILEGES ON lolas_database.* TO 'lolas_owner'@'localhost';
FLUSH PRIVILEGES;

REVOKE ALL PRIVILEGES ON lolas_database.* FROM 'lolas_owner'@'localhost';
FLUSH PRIVILEGES;
```

### 5. How to SHOW, DELETE & CREATE DATABASES & SELECT DATABASES
```
SHOW DATABASES; // to show available databases
CREATE DATABASE lolas_database; // to create a database named lolas_database
USE lolas_database; // to select a database 
DROP DATABASE lolas_database; // to delete a database
```

### 6. How to CREATE a TABLE with Columns and their data types
```
CREATE TABLE lolas_activities (
    activity_ID INT PRIMARY KEY,
    activity_name VARCHAR(50),
    location VARCHAR(50),
    date DATE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### 7. How to SHOW, DELETE & DROP Tables
```
SHOW TABLES;
DELETE FROM lolas_activities WHERE activity_ID = 1;
DROP TABLE lolas_activities;
```

### 8. How to Insert ROWS & RECORDS (single and multiple)
```
INSERT INTO mytable (column1, column2, column3, ...) VALUES (value1, value2, value3, ...);
INSERT INTO mytable (column1, column2, column3, ...)
VALUES
(value1_row1, value2_row1, value3_row1, ...),
(value1_row2, value2_row2, value3_row2, ...),
...;

INSERT INTO lola_activities (activity_name, location, date, duration_minutes)
VALUES
('Sleeping', 'Everywhere', '2024-01-22', 90),
('Running', 'Beach', '2024-01-23', 2);
```

### 9. How to SELECT with the WHERE Clause
```
SELECT column1, column2, ...
FROM mytable
WHERE condition;

SELECT activity_name, location, date
FROM lolas_activities
WHERE duration_minutes > 20;
```

### 10. How to SELECT with the WHERE Clause using a range

```
SELECT product_id, product_name, price
FROM products
WHERE price BETWEEN 50 AND 100;

SELECT activity_name, location, date
FROM lolas_activities
WHERE duration_minutes BETWEEN 60 and 90;
```

### 11. How to DELETE an individual ROW
```
DELETE FROM mytable
WHERE condition;

DELETE FROM lola_activites
WHERE activity_ID = 1;
```

### 12. How to UPDATE a ROW
```
UPDATE mytable
SET column1 = value1, column2 = value2, ...
WHERE condition;

UPDATE lolas_activities
SET duration_minutes = 90
WHERE activity_ID = 2;
```

### 13. How to add a new column and modify it
```
-- Step 1: Add a new column 'bonus' with default value 0
ALTER TABLE employees
ADD COLUMN bonus INT DEFAULT 0;

ALTER TABLE lolas_activites
ADD COLUMN weather VARCHAR(50) DEFAULT 'Sunny';

-- Step 2: Update the 'bonus' values for specific employees
UPDATE employees
SET bonus = 5000
WHERE employee_id IN (1, 2, 3);

UPDATE lolas_activities
SET weather = 'Rainy'
WHERE activity_ID = 1;
```

### 14. How to Order by and use Distinct
```
SELECT column1, column2, ...
FROM mytable
ORDER BY column1 [ASC|DESC], column2 [ASC|DESC], ...;

SELECT DISTINCT column1, column2, ...
FROM mytable
ORDER BY column1 [ASC|DESC], column2 [ASC|DESC], ...;

SELECT activity_name, location, date
FROM lolas_activities
ORDER BY date DESC, duration_minutes ASC;
```

### 15. How to Concatenate Columns
```
SELECT CONCAT(first_name, ', ', last_name) AS full_name
FROM employees;

SELECT CONCAT(activity_name, ' at ' , location) AS activity_details
FROM lolas_activites;
```

### 16.  How to Select Distinct Rows
```
SELECT DISTINCT column1, column2, ...
FROM mytable;

SELECT DISTINCT location
FROM lolas_activities;
```

### 17. How to use LIKE to Search
```
SELECT column1, column2, ...
FROM mytable
WHERE column_name LIKE pattern;

SELECT activity_name, location
FROM locals_activities
WHERE location LIKE %Park%;
```

### 18. How Select using IN
```
SELECT column1, column2, ...
FROM mytable
WHERE column_name IN (value1, value2, ...);

SELECT activity_name, location
FROM lolas_activities
WHERE activity_name IN ('Running', 'Sleeping');
```

### 19. How to Create & Remove Index
```
CREATE INDEX index_name
ON table_name (column1, column2, ...);

DROP INDEX index_name
ON table_name;

CREATE INDEX activity_name_index
ON lolas_activities (activity_name);

DROP INDEX activity_name_index
ON lolas_activites;
```

### Create Two Tables in the LOLA database demonstrating a one-to-many relationship with Primary Key and Foreign Key

```
CREATE TABLE lolas_Owners (
    owner_ID INT PRIMARY KEY,
    owner_name VARCHAR(50),
    contact_number VARCHAR(15)
);

CREATE TABLE lolas_Activities (
    activity_ID INT PRIMARY KEY,
    activity_name VARCHAR(50),
    location VARCHAR(50),
    date DATE,
    duration_minutes INT,
    owner_ID INT,
    FOREIGN KEY (owner_ID) REFERENCES lolas_Owners(owner_id)
);
```

### Show how to use INNER JOIN

SELECT lolas_Activities.activity_ID, lolas_Activities.activity_name, lolas_Owners.owner_name, lolas_Owners.contact_number
FROM lolas_Activities
INNER JOIN lolas_Owners ON lolas_Activities.owner_ID = lolas_Owners.owner_ID;

### Show how to JOIN Multiple Tables 

CREATE TABLE pets (
    pet_ID INT PRIMARY KEY,
    pet_name VARCHAR(50),
    owner_ID INT,
    FOREIGN KEY (owner_ID) REFERENCES lolas_Owners(owner_ID)
);

SELECT lolas_Activities.activity_ID, lolas_Activities.activity_name, lolas_Owners.owner_name, lolas_Pets.pet_name
FROM lolas_Activities
INNER JOIN lolas_Owners ON lolas_Activities.owner_ID = lolas_Owners.owner_ID
LEFT JOIN lolas_Pets ON lolas_Owners.owner_ID = lola_Pets.owner_ID;

