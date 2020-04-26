
Create students table:

```CREATE TABLE students(
student_id serial PRIMARY KEY,
first_name VARCHAR(50) NOT NULL,
last_name VARCHAR(50) NOT NULL,
homeroom_number INTEGER,
phone VARCHAR(15) UNIQUE NOT NULL,
email VARCHAR(355) UNIQUE,
graduation INTEGER
)
```

Create teachers table:

```
CREATE TABLE teachers(
teacher_id serial PRIMARY KEY,
first_name VARCHAR(50) NOT NULL,
last_name VARCHAR(50) NOT NULL,
homeroom_number INTEGER,
department VARCHAR(50),
email VARCHAR(355) UNIQUE,
phone VARCHAR(15) UNIQUE NOT NULL
)
```

Inserts:

```
INSERT INTO students(first_name, last_name, phone, graduation, homeroom_number)
VALUES('Mark', 'Watney', '777-555-1234', 2035, 5);
```

```
INSERT INTO teachers(first_name, last_name, homeroom_number, department, email, phone)
VALUES ('Jonas', 'Salk', 5, 'Biology', 'jsalk@school.org', '777-555-4321');
```
