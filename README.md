# byteXLsql-mst_practical
output 
<img width="1796" height="773" alt="image" src="https://github.com/user-attachments/assets/dbcf6110-6e5a-4b07-ae4d-66308c4c909a" />


ouput
-- Drop the view first (if exists)
DROP VIEW IF EXISTS score_high;

-- Drop tables (marks first because it depends on student)
DROP TABLE IF EXISTS marks;
DROP TABLE IF EXISTS student;



-- 1. Create Student table
CREATE TABLE student (
    s_id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    department VARCHAR(50) NOT NULL
);

-- 2. Create Marks table
CREATE TABLE marks (
    mark_id INT PRIMARY KEY,
    s_id INT,
    subject VARCHAR(50) NOT NULL,
    marks INT CHECK (marks >= 0 AND marks <= 100),
    FOREIGN KEY (s_id) REFERENCES student(s_id)
);

-- 3. Insert records into Student table
INSERT INTO student (s_id, name, department) VALUES
(1, 'Prabhat', 'CSE'),
(2, 'Ambarish', 'ECE'),
(3, 'Eshant', 'IT'),
(4, 'Rohit', 'CSE'),
(5, 'Vishal', 'ME');
-- See all students
SELECT * FROM student;

-- 4. Insert records into Marks table (8 records)
INSERT INTO marks (mark_id, s_id, subject, marks) VALUES
(101, 1, 'Maths', 85),
(102, 1, 'Physics', 72),
(103, 2, 'Electronics', 90),
(104, 2, 'Maths', 65),
(105, 3, 'DBMS', 88),
(106, 4, 'OS', 78),
(107, 5, 'Thermo', 95),
(108, 3, 'Networks', 55);

-- See all marks
SELECT * FROM marks;

-- 5. Create a view for students scoring more than 80
CREATE VIEW score_high AS
SELECT s.s_id, s.name, s.department, m.subject, m.marks
FROM student s
JOIN marks m ON s.s_id = m.s_id
WHERE m.marks > 80;

-- See high scorers (>80) from the view
SELECT * FROM score_high;


-- 6. Use the view to show department-wise entries (not average)
SELECT department, name, subject, marks
FROM score_high
ORDER BY department, name;
