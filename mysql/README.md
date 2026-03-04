**# College Club Membership Management System
##  Database Normalization (1NF → 3NF)**

---

## Overview

Normalization is a database design technique used to organize data efficiently.  
It reduces data redundancy and prevents problems such as:

- Insert anomalies  
- Update anomalies  
- Delete anomalies  

The process divides a large table into smaller related tables using primary and foreign keys.

---

## Initial Data Structure (Unnormalized Form)


This table stores **Student + Club + Membership** information together.

---

## Problems Identified

### 1️.Data Redundancy
- Student details are repeated multiple times.
- Club details are also repeated.
- Wastes storage and may cause inconsistency.

### 2️.Update Anomaly
- If a club room changes, it must be updated in many rows.
- Missing one row causes incorrect data.

### 3️.Insert Anomaly
- Cannot add a new club unless a student joins it.
- Cannot add a student without club membership.

---

## Why It Is Not Normalized

- Multiple entities stored in one table.
- Repeated data exists.
- Functional dependencies are unclear.
- Causes insert, update, and delete anomalies.

---

# Step 1: Create Unnormalized Table

```sql
CREATE TABLE DATA (
    ID INT AUTO_INCREMENT PRIMARY KEY,
    StudentID INT,
    StudentName VARCHAR(50),
    Email VARCHAR(50),
    ClubName VARCHAR(50),
    ClubRoom VARCHAR(50),
    ClubMentor VARCHAR(50),
    JoinDate DATE
);



INSERT INTO DATA (StudentID, StudentName, Email, ClubName, ClubRoom, ClubMentor, JoinDate) VALUES
(1, 'Asha', 'asha@email.com', 'Music Club', 'R101', 'Mr. Raman', '2024-01-10'),
(2, 'Bikash', 'bikash@email.com', 'Sports Club', 'R202', 'Ms. Sita', '2024-01-12'),
(1, 'Asha', 'asha@email.com', 'Sports Club', 'R202', 'Ms. Sita', '2024-01-15'),
(3, 'Nisha', 'nisha@email.com', 'Music Club', 'R101', 'Mr. Raman', '2024-01-20'),
(4, 'Rohan', 'rohan@email.com', 'Drama Club', 'R303', 'Mr. Kiran', '2024-01-18'),
(5, 'Suman', 'suman@email.com', 'Music Club', 'R101', 'Mr. Raman', '2024-01-22'),
(2, 'Bikash', 'bikash@email.com', 'Drama Club', 'R303', 'Mr. Kiran', '2024-01-25'),
(6, 'Pooja', 'pooja@email.com', 'Sports Club', 'R202', 'Ms. Sita', '2024-01-27'),
(3, 'Nisha', 'nisha@email.com', 'Coding Club', 'Lab1', 'Mr. Anil', '2024-01-28'),
(7, 'Aman', 'aman@email.com', 'Coding Club', 'Lab1', 'Mr. Anil', '2024-01-30');

+-----------+-------------+------------------+-------------+----------+------------+------------+
| StudentID | StudentName | Email            | ClubName    | ClubRoom | ClubMentor | JoinDate   |
+-----------+-------------+------------------+-------------+----------+------------+------------+
|         1 | Asha        | asha@email.com   | Music Club  | R101     | Mr. Raman  | 2024-01-10 |
|         1 | Asha        | asha@email.com   | Sports Club | R202     | Ms. Sita   | 2024-01-15 |
|         2 | Bikash      | bikash@email.com | Drama Club  | R303     | Mr. Kiran  | 2024-01-25 |
|         2 | Bikash      | bikash@email.com | Sports Club | R202     | Ms. Sita   | 2024-01-12 |
|         3 | Nisha       | nisha@email.com  | Coding Club | Lab1     | Mr. Anil   | 2024-01-28 |
|         3 | Nisha       | nisha@email.com  | Music Club  | R101     | Mr. Raman  | 2024-01-20 |
|         4 | Rohan       | rohan@email.com  | Drama Club  | R303     | Mr. Kiran  | 2024-01-18 |
|         5 | Suman       | suman@email.com  | Music Club  | R101     | Mr. Raman  | 2024-01-22 |
|         6 | Pooja       | pooja@email.com  | Sports Club | R202     | Ms. Sita   | 2024-01-27 |
|         7 | Aman        | aman@email.com   | Coding Club | Lab1     | Mr. Anil   | 2024-01-30 |
+-----------+-------------+------------------+-------------+----------+------------+------------+

CREATE TABLE Student (
    StudentID INT PRIMARY KEY,
    StudentName VARCHAR(50),
    Email VARCHAR(50)
);

CREATE TABLE Club (
    ClubID INT PRIMARY KEY,
    ClubName VARCHAR(50),
    ClubRoom VARCHAR(50),
    ClubMentor VARCHAR(50)
);

CREATE TABLE Membership (
    StudentID INT,
    ClubID INT,
    JoinDate DATE,
    PRIMARY KEY (StudentID, ClubID),
    FOREIGN KEY (StudentID) REFERENCES Student(StudentID),
    FOREIGN KEY (ClubID) REFERENCES Club(ClubID)
);

INSERT INTO Student VALUES
(8, 'Srijan', 'srijan@email.com');

INSERT INTO Club VALUES
(5, 'Hacking Club', 'R699', 'Ms. Piya');

SELECT * FROM Student;
+-----------+-------------+------------------+
| StudentID | StudentName | Email            |
+-----------+-------------+------------------+
|         1 | Asha        | asha@email.com   |
|         2 | Bikash      | bikash@email.com |
|         3 | Nisha       | nisha@email.com  |
|         4 | Rohan       | rohan@email.com  |
|         5 | Suman       | suman@email.com  |
|         6 | Pooja       | pooja@email.com  |
|         7 | Aman        | aman@email.com   |
+-----------+-------------+------------------+

SELECT * FROM Club;
+--------+-------------+----------+------------+
| ClubID | ClubName    | ClubRoom | ClubMentor |
+--------+-------------+----------+------------+
|    101 | Music Club  | R101     | Mr. Raman  |
|    202 | Sports Club | R202     | Ms. Sita   |
|    303 | Drama Club  | R303     | Mr. Kiran  |
|    401 | Coding Club | Lab1     | Mr. Anil   |
+--------+-------------+----------+------------+

SELECT * FROM Membership;
+--------------+-----------+--------+------------+
| MembershipID | StudentID | ClubID | JoinDate   |
+--------------+-----------+--------+------------+
|            1 |         1 |    101 | 2024-01-10 |
|            2 |         2 |    202 | 2024-01-12 |
|            3 |         1 |    202 | 2024-01-15 |
|            4 |         3 |    101 | 2024-01-20 |
|            5 |         4 |    303 | 2024-01-18 |
|            6 |         5 |    101 | 2024-01-22 |
|            7 |         2 |    303 | 2024-01-25 |
|            8 |         6 |    202 | 2024-01-27 |
|            9 |         3 |    401 | 2024-01-28 |
|           10 |         7 |    401 | 2024-01-30 |
+--------------+-----------+--------+------------+

SELECT s.StudentName, c.ClubName, m.JoinDate
FROM Membership m
JOIN Student s ON m.StudentID = s.StudentID
JOIN Club c ON m.ClubID = c.ClubID;
    -> JOIN Club c ON sc.ClubID = c.ClubID;
+-------------+-------------+------------+
| StudentName | ClubName    | JoinDate   |
+-------------+-------------+------------+
| Asha        | Music Club  | 2024-01-10 |
| Nisha       | Music Club  | 2024-01-20 |
| Suman       | Music Club  | 2024-01-22 |
| Bikash      | Sports Club | 2024-01-12 |
| Asha        | Sports Club | 2024-01-15 |
| Pooja       | Sports Club | 2024-01-27 |
| Rohan       | Drama Club  | 2024-01-18 |
| Bikash      | Drama Club  | 2024-01-25 |
| Nisha       | Coding Club | 2024-01-28 |
| Aman        | Coding Club | 2024-01-30 |
+-------------+-------------+------------+

