# College Club Membership Management System

## Database Normalization (1NF → 3NF)

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

This table stores *Student + Club + Membership* information together in one table.

---

## Problems Identified

### 1. Data Redundancy
- Student details are repeated multiple times.
- Club details are also repeated.
- Wastes storage and may cause inconsistency.

### 2. Update Anomaly
- If a club room changes, it must be updated in many rows.
- Missing one row causes incorrect data.

### 3. Insert Anomaly
- Cannot add a new club unless a student joins it.
- Cannot add a student without club membership.

---

## Why It Is Not Normalized

- Multiple entities stored in one table.
- Repeated data exists.
- Functional dependencies are unclear.
- Causes insert, update, and delete anomalies.

---

# Step 1: Create Unnormalized Table (1NF)

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
```


### Insert Sample Data

```sql
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
```
```
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
```

---

# Step 2: Convert to 2NF & 3NF (Normalized Design)

The large table is divided into three smaller related tables:

- Student
- Club
- Membership

---

## Student Table

```sql
CREATE TABLE Student (
    StudentID INT PRIMARY KEY,
    StudentName VARCHAR(50),
    Email VARCHAR(50)
);
```


---

## Club Table

```sql
CREATE TABLE Club (
    ClubID INT PRIMARY KEY,
    ClubName VARCHAR(50),
    ClubRoom VARCHAR(50),
    ClubMentor VARCHAR(50)
);
```

---

## Membership Table

```sql
CREATE TABLE Membership (
    StudentID INT,
    ClubID INT,
    JoinDate DATE,
    PRIMARY KEY (StudentID, ClubID),
    FOREIGN KEY (StudentID) REFERENCES Student(StudentID),
    FOREIGN KEY (ClubID) REFERENCES Club(ClubID)
);
```

---

# Insert Data into Normalized Tables

### Insert Students

```sql
INSERT INTO Student VALUES
(1, 'Asha', 'asha@email.com'),
(2, 'Bikash', 'bikash@email.com'),
(3, 'Nisha', 'nisha@email.com'),
(4, 'Rohan', 'rohan@email.com'),
(5, 'Suman', 'suman@email.com'),
(6, 'Pooja', 'pooja@email.com'),
(7, 'Aman', 'aman@email.com');
```

---

### Insert Clubs

```sql
INSERT INTO Club VALUES
(101, 'Music Club', 'R101', 'Mr. Raman'),
(202, 'Sports Club', 'R202', 'Ms. Sita'),
(303, 'Drama Club', 'R303', 'Mr. Kiran'),
(401, 'Coding Club', 'Lab1', 'Mr. Anil');
```

---

### Insert Membership Records

```sql
INSERT INTO Membership VALUES
(1, 101, '2024-01-10'),
(2, 202, '2024-01-12'),
(1, 202, '2024-01-15'),
(3, 101, '2024-01-20'),
(4, 303, '2024-01-18'),
(5, 101, '2024-01-22'),
(2, 303, '2024-01-25'),
(6, 202, '2024-01-27'),
(3, 401, '2024-01-28'),
(7, 401, '2024-01-30');
```

---

# Display Data

```sql
SELECT * FROM Student;
SELECT * FROM Club;
SELECT * FROM Membership;
```

```
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
```

```
+--------+-------------+----------+------------+
| ClubID | ClubName    | ClubRoom | ClubMentor |
+--------+-------------+----------+------------+
|    101 | Music Club  | R101     | Mr. Raman  |
|    202 | Sports Club | R202     | Ms. Sita   |
|    303 | Drama Club  | R303     | Mr. Kiran  |
|    401 | Coding Club | Lab1     | Mr. Anil   |
+--------+-------------+----------+------------+
```

```
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
```

---

# Join Tables to View Final Result

```sql
SELECT s.StudentName, c.ClubName, m.JoinDate
FROM Membership m
JOIN Student s ON m.StudentID = s.StudentID
JOIN Club c ON m.ClubID = c.ClubID;
```

```
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
```

---

# Final Result

The database is now fully normalized (3NF):

- No redundant data
- No update anomalies
- No insert anomalies
- Better data integrity
- Proper use of Primary and Foreign Keys

---

## Conclusion

The College Club Membership database was successfully transformed from an unnormalized structure into a fully normalized design (1NF → 3NF).  

This improves efficiency, consistency, and scalability of the database system.

Author: Srijan Shrestha

Bachelor of Cybersecurity and Ethical Hacking
