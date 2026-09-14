# U4 – Student Course Management Database

## 📌 Project Overview

This project is a **Student Course Management Database** developed using **MySQL**.

The database manages:

* Students
* Courses
* Student Enrollments
* Instructors
* Course-Instructor relationships
* User creation and database privileges
* GRANT and REVOKE operations
* SQL JOIN operations

The project demonstrates important **DBMS concepts**, including **Primary Keys, Foreign Keys, Composite Keys, Constraints, DML, DDL, JOINs, User Management, GRANT, and REVOKE**.

---

## 🗄️ Database Information

**Database Name:** `u4`

```sql
CREATE DATABASE u4;
USE u4;
```

---

## 📊 Database Tables

The database contains five tables:

### 1. Students

Stores information about students.

| Column      | Data Type   | Description            |
| ----------- | ----------- | ---------------------- |
| StudentID   | INT         | Primary key            |
| StudentName | VARCHAR(50) | Student name           |
| Major       | VARCHAR(50) | Student's branch/major |

Example:

```text
101 | Rahul | CSE
102 | Priya | AIML
103 | Arjun | ECE
104 | Sneha | AIML
```

---

### 2. Courses

Stores information about available courses.

| Column     | Data Type    | Description       |
| ---------- | ------------ | ----------------- |
| CourseID   | INT          | Primary key       |
| CourseName | VARCHAR(100) | Course name       |
| Credits    | INT          | Number of credits |

Example:

```text
201 | Database Management Systems | 4
202 | Operating Systems            | 4
203 | Machine Learning             | 3
204 | Computer Networks            | 3
```

---

### 3. Enrollments

Stores which students are enrolled in which courses.

| Column         | Data Type | Description     |
| -------------- | --------- | --------------- |
| StudentID      | INT       | Foreign key     |
| CourseID       | INT       | Foreign key     |
| EnrollmentDate | DATE      | Enrollment date |

The combination of `StudentID` and `CourseID` forms a **Composite Primary Key**.

```sql
PRIMARY KEY (StudentID, CourseID)
```

Relationships:

```text
Students ───────< Enrollments >─────── Courses
```

---

### 4. Instructors

Stores instructor information.

| Column         | Data Type   | Description     |
| -------------- | ----------- | --------------- |
| InstructorID   | INT         | Primary key     |
| InstructorName | VARCHAR(50) | Instructor name |
| Phone          | VARCHAR(15) | Contact number  |

Example:

```text
301 | Dr. Kumar  | 9876543210
302 | Dr. Anitha | 9876543211
303 | Dr. Ramesh | 9876543212
```

---

### 5. Course_Instructors

Connects courses with instructors.

| Column       | Data Type | Description |
| ------------ | --------- | ----------- |
| CourseID     | INT       | Foreign key |
| InstructorID | INT       | Foreign key |

The combination of `CourseID` and `InstructorID` is the **Composite Primary Key**.

Relationship:

```text
Courses ───────< Course_Instructors >─────── Instructors
```

---

# 🔑 Database Relationships

The overall database relationship can be represented as:

```text
                 ┌──────────────┐
                 │   Students   │
                 └──────┬───────┘
                        │
                        │ StudentID
                        ▼
                 ┌──────────────┐
                 │ Enrollments  │
                 └──────┬───────┘
                        │
                        │ CourseID
                        ▼
                 ┌──────────────┐
                 │   Courses    │
                 └──────┬───────┘
                        │
                        │ CourseID
                        ▼
              ┌────────────────────┐
              │ Course_Instructors │
              └──────────┬─────────┘
                         │
                         │ InstructorID
                         ▼
                  ┌──────────────┐
                  │ Instructors  │
                  └──────────────┘
```

---

# 🛠️ Technologies Used

* **MySQL**
* SQL
* MySQL Workbench

---

# 📚 SQL Concepts Demonstrated

## 1. Database Creation

```sql
CREATE DATABASE u4;
USE u4;
```

Creates and selects the `u4` database.

---

## 2. Table Creation

The project uses `CREATE TABLE` to create the required tables.

Example:

```sql
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    StudentName VARCHAR(50) NOT NULL,
    Major VARCHAR(50)
);
```

---

## 3. Primary Key

A primary key uniquely identifies every record.

Example:

```sql
StudentID INT PRIMARY KEY
```

---

## 4. Foreign Key

Foreign keys establish relationships between tables.

Example:

```sql
FOREIGN KEY (StudentID)
REFERENCES Students(StudentID)
```

---

## 5. Composite Primary Key

The `Enrollments` table uses two columns together as its primary key.

```sql
PRIMARY KEY (StudentID, CourseID)
```

This prevents the same student from being enrolled in the same course more than once.

---

# 🔎 SQL Queries

## Find AIML Students

```sql
SELECT StudentID, StudentName
FROM Students
WHERE Major = 'AIML';
```

Expected result:

```text
102 | Priya
104 | Sneha
```

---

## Display Students and Their Courses

```sql
SELECT S.StudentID,
       S.StudentName,
       C.CourseName
FROM Students S
JOIN Enrollments E
    ON S.StudentID = E.StudentID
JOIN Courses C
    ON E.CourseID = C.CourseID;
```

This demonstrates the use of **INNER JOIN**.

---

## Display Courses and Instructors

```sql
SELECT C.CourseName,
       I.InstructorName
FROM Courses C
JOIN Course_Instructors CI
    ON C.CourseID = CI.CourseID
JOIN Instructors I
    ON CI.InstructorID = I.InstructorID;
```

This displays which instructor teaches each course.

---

## Display Enrollment Details

```sql
SELECT S.StudentName,
       C.CourseName,
       E.EnrollmentDate
FROM Students S
JOIN Enrollments E
    ON S.StudentID = E.StudentID
JOIN Courses C
    ON E.CourseID = C.CourseID;
```

This displays:

* Student name
* Course name
* Enrollment date

---

# 👤 User Management

A MySQL user is created using:

```sql
CREATE USER 'u4_user'@'localhost'
IDENTIFIED BY 'U4user@123';
```

Username:

```text
u4_user
```

Password:

```text
U4user@123
```

---

# 🔐 GRANT Privileges

The `GRANT` command provides permissions to a database user.

### Give SELECT permission

```sql
GRANT SELECT
ON u4.Students
TO 'u4_user'@'localhost';
```

The user can now view data from the `Students` table.

---

### Give SELECT, INSERT and UPDATE

```sql
GRANT SELECT, INSERT, UPDATE
ON u4.Students
TO 'u4_user'@'localhost';
```

The user can:

* View records
* Insert records
* Update records

---

### Give all privileges

```sql
GRANT ALL PRIVILEGES
ON u4.*
TO 'u4_user'@'localhost';
```

This grants all available privileges on the `u4` database.

---

# 🚫 REVOKE Privileges

`REVOKE` removes permissions from a user.

### Remove UPDATE

```sql
REVOKE UPDATE
ON u4.Students
FROM 'u4_user'@'localhost';
```

### Remove INSERT

```sql
REVOKE INSERT
ON u4.Students
FROM 'u4_user'@'localhost';
```

### Remove SELECT

```sql
REVOKE SELECT
ON u4.Students
FROM 'u4_user'@'localhost';
```

After removing SELECT, the user can no longer read the `Students` table through that privilege.

---

# 🗑️ DELETE Operation

To delete Student 101 safely:

```sql
DELETE FROM Enrollments
WHERE StudentID = 101;

DELETE FROM Students
WHERE StudentID = 101;
```

The enrollment records must be removed first because they reference the student through a foreign key.

---

# 📁 Project Structure

Recommended project folder:

```text
U4-Student-Course-Management/
│
├── README.md
│
└── u4_database.sql
```

### `u4_database.sql`

Contains:

* Database creation
* Table creation
* Data insertion
* SELECT queries
* JOIN queries
* User creation
* GRANT operations
* REVOKE operations

### `README.md`

Contains the project documentation and explanation.

---

# ▶️ How to Run the Project

## Step 1: Open MySQL Workbench

Open MySQL Workbench and connect to your MySQL server.

## Step 2: Open the SQL file

Open:

```text
u4_database.sql
```

## Step 3: Execute the SQL

Run the SQL statements in order.

## Step 4: Select the database

```sql
USE u4;
```

## Step 5: Check the tables

```sql
SHOW TABLES;
```

You should see:

```text
Course_Instructors
Courses
Enrollments
Instructors
Students
```

## Step 6: Display the data

```sql
SELECT * FROM Students;
SELECT * FROM Courses;
SELECT * FROM Enrollments;
SELECT * FROM Instructors;
SELECT * FROM Course_Instructors;
```

---

# 🎯 Learning Outcomes

After completing this project, the following DBMS concepts are demonstrated:

* Database creation
* Table creation
* Primary keys
* Foreign keys
* Composite primary keys
* NOT NULL constraints
* Data insertion
* Data retrieval
* Data deletion
* WHERE clause
* INNER JOIN
* Multiple-table queries
* Database relationships
* User creation
* Access control
* GRANT
* REVOKE
* Referential integrity

---

# ⚠️ Important Note

The database name used throughout this project is:

```text
u4
```

Therefore, privilege commands should use:

```sql
ON u4.Students
```

or:

```sql
ON u4.*
```

and **not**:

```sql
ON University.Students
```

unless a separate database named `University` has been created.

---

# 👨‍💻 Project Information

**Project:** Student Course Management Database

**Database:** MySQL

**Database Name:** `u4`

**Main Concepts:** SQL, Joins, Keys, Constraints, User Privileges, GRANT and REVOKE
