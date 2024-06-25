## Creating a Database and Tables

```sql

-- Create a database
CREATE DATABASE school;

-- Use the database
USE school;

-- Create a student table
CREATE TABLE students (
    student_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    age INT,
    grade VARCHAR(10)
);


```

## Inserting Data

```sql
-- Insert records into students table
INSERT INTO students (first_name, last_name, age, grade)
VALUES
('John', 'Doe', 18, '12th'),
('Jane', 'Smith', 17, '11th'),
('Emily', 'Jones', 16, '10th');


```

## Querying Data

```sql
-- Select all records
SELECT * FROM students;

-- Select specific columns
SELECT first_name, last_name FROM students;

-- Select records with a condition
SELECT * FROM students WHERE age > 16;


```

## Updating Data

```sql
-- Update age for a student
UPDATE students
SET age = 19
WHERE student_id = 1;

```

## Delete Data

```sql
-- Delete a student record
DELETE FROM students
WHERE student_id = 3;


```
## Example: Aggregate Functions

```sql
-- Get the average age of students
SELECT AVG(age) AS average_age FROM students;

-- Count the number of students
SELECT COUNT(*) AS student_count FROM students;


```

## Example: Grouping Data

```sql
-- Group by grade and count students in each grade
SELECT grade, COUNT(*) AS student_count
FROM students
GROUP BY grade;

```

## Creating Indexes

```sql
-- Create an index on the last_name column
CREATE INDEX idx_last_name ON students(last_name);

```

## Transactions 
Ensure data integrity using transactions.

```sql
-- Start a transaction
START TRANSACTION;

-- Multiple operations
UPDATE students SET age = 18 WHERE student_id = 2;
DELETE FROM students WHERE student_id = 1;

-- Commit the transaction
COMMIT;

-- If something goes wrong, you can roll back
-- ROLLBACK;

```

