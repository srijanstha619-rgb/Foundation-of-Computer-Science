
---

# Abstract

This project explores fundamental computing concepts including **secure data exchange, computational complexity, and database normalization**.

The project explains encoding formats such as **ASCII, Base64, URL Encoding, and Hexadecimal**, and their role in protocols like **HTTPS, TLS, REST APIs, and OAuth**. It also analyzes a **classroom seating arrangement problem** to demonstrate **P vs NP complexity**, comparing brute-force and heuristic solutions. Finally, it demonstrates **database normalization** using a **College Club Membership system** with ER modeling and SQL queries.

---

# 1. Secure Data Exchange and Encoding

## 1.1 Introduction

Modern networks require both **encoding and encryption**.

* **Encoding** converts data into safe text formats for transmission.
* **Encryption (TLS/HTTPS)** ensures confidentiality and integrity.

Encoding is widely used in:

* REST APIs
* OAuth authentication
* Email attachments (SMTP)
* Web forms
* HTTP query parameters

However, encoding **does not provide security** and must be used together with encryption.

---

# 1.2 Common Encoding Formats

## ASCII Encoding

ASCII represents characters using **7-bit binary values (0-127)**.

### Python Example

```python
text = "Hello"

encoded = text.encode("ascii")
decoded = encoded.decode("ascii")

print("Encoded:", encoded)
print("Decoded:", decoded)
```

### Output

```
Encoded: b'Hello'
Decoded: Hello
```

---

#  Base64 Encoding

##  Objective

To demonstrate encoding of data before transmission and decoding after reception using two Docker containers connected via a custom network.

---

##  Step 1 – Create Docker Network

```bash
docker network create blue
docker network ls
```

---

##  Step 2 – Start Server Container

```bash
docker run -it --name server --network blue python:3.12-slim bash
```

---

##  Step 3 – Install Required Packages (Server)

```bash
apt update
apt install -y wget iputils-ping
```

---

##  Step 4 – Create Original File (Server)

```bash
echo "This is secret data for Base64 Demo" > dem.txt
ls
```

---

##  Step 5 – Encode File Using Base64 (Server)

```bash
base64 dem.txt > encoded.txt
cat encoded.txt
```

Example encoded output:

```
VGhpcyBpcyBzZWNyZXQgZGF0YSBmb3IgQmFzZTY0IERlbW8K
```

---

##  Step 6 – Check Server IP (Windows CMD)

```bash
docker inspect -f "{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}" server
```

Example output:

```
172.18.0.2
```

---

##  Step 7 – Start HTTP Server (Server)

```bash
python -m http.server 8082
```

---

##  Step 8 – Start Client Container

(Open new CMD window)

```bash
docker run -it --name client --network blue python:3.12-slim bash
```

---

##  Step 9 – Install Packages (Client)

```bash
apt update
apt install -y wget iputils-ping
```

---

##  Step 10 – Test Network Connectivity (Client)

Using container name:

```bash
ping server
```

Using IP address:

```bash
ping 172.18.0.2
```

---

##  Step 11 – Download Encoded File (Client)

Using hostname:

```bash
wget http://server:8082/encoded.txt
```

Using IP:

```bash
wget http://172.18.0.2:8082/encoded.txt
```

---

##  Step 12 – Decode File (Client)

```bash
base64 -d encoded.txt > decoded.txt
cat decoded.txt
```

Expected output:

```
This is secret data for Base64 Demo
```

---

##  What Task 1 Demonstrates

- Docker networking  
- Container-to-container communication  
- DNS resolution within Docker  
- Base64 encoding before transmission  
- File transfer via HTTP  
- Decoding after reception  
- Difference between HTTP and HTTPS  

---

---

## URL Encoding

URL encoding converts unsafe characters into **percent encoded values**.


## Hexadecimal Encoding

Hexadecimal encoding represents binary data using **base-16 numbers (0-9, A-F)**.

### Python Example

```python
text = "Hello"

encoded = text.encode().hex()
decoded = bytes.fromhex(encoded).decode()

print("Hex:", encoded)
print("Decoded:", decoded)
```

### Output

```
Hex: 48656c6c6f
Decoded: Hello
```

---

# 1.3 Encoding in HTTP Communication

Encoding ensures compatibility when transmitting data through HTTP.

| Encoding     | Purpose                  |
| ------------ | ------------------------ |
| Base64       | File transfer and tokens |
| URL Encoding | Query parameters         |
| Hexadecimal  | Hash values              |

Security is provided by **TLS encryption**, not encoding.

---

# 1.4 File Transmission Between Docker Containers

Docker containers communicate using **shared networks**.

### Create Network

```bash
docker network create srijan
```

### Run Container

```bash
docker run -it --name rm --network srijan ubuntu
```

### Create File

```bash
echo "Hello from container" > file1.txt
```

### Start HTTP Server

```bash
python3 -m http.server 8086
```

### Download File from Another Container

```bash
wget http://rm:8086/file1.txt
```

### Output

```
Connecting to rm:8086
HTTP request sent, awaiting response... 200 OK
Saving to: ‘file1.txt’

file1.txt saved
```

---


# 2. Computational Complexity Analysis

## 2.1 Introduction

Computational complexity studies how the **difficulty of solving problems increases as input size grows**.

Example: **Classroom seating arrangement**

Constraints:

* Friends cannot sit together
* Students from the same city cannot sit together

Possible arrangements:

```
n! permutations
```

---

## 2.2 P vs NP Problems

### P Problems

Solved efficiently in polynomial time.

Examples:

* Sorting numbers
* Verifying seating arrangement

### NP Problems

Solutions are easy to verify but hard to compute.

Examples:

* Graph coloring
* Constraint satisfaction

---

## 2.3 Brute Force Method

Brute force tests **all permutations**.

| Students | Possible Arrangements |
| -------- | --------------------- |
| 5        | 120                   |
| 8        | 40,320                |
| 10       | 3,628,800             |

Time complexity:

```
O(n!)
```

---

## 2.4 Heuristic Method

Heuristic algorithms use **smart strategies** to reduce computation.

Examples:

* Seat students with many restrictions first
* Separate students from same city early

Advantages:

* Faster
* Practical

Disadvantage:

* May not always produce optimal solution

---

# 3. Database Normalization

Normalization organizes relational databases to **reduce redundancy and improve data integrity**.

Normalization stages:

1. First Normal Form (1NF)
2. Second Normal Form (2NF)
3. Third Normal Form (3NF)

---

# 3.1 Scenario – Club Membership System

Original table stores student and club information together causing:

* Data redundancy
* Update anomaly
* Insert anomaly
* Delete anomaly

---

# 3.2 Normalized Database Structure

## ER Diagram

```
Student
--------
StudentID (PK)
StudentName
Email

Club
--------
ClubID (PK)
ClubName
ClubRoom
ClubMentor

Membership
--------
MembershipID (PK)
StudentID (FK)
ClubID (FK)
JoinDate
```

Relationship:

```
Student 1 --- * Membership * --- 1 Club
```

---

#  3.3 Database Normalisation (1NF to 3NF)



---

#  Overview

Task 3 demonstrates the process of database normalisation from:

- First Normal Form (1NF)  
- Second Normal Form (2NF)  
- Third Normal Form (3NF)  

The goal is to remove redundancy, eliminate partial dependencies, and ensure proper relational database structure using primary and foreign keys.

---

#  Step 1 – First Normal Form (1NF)

In 1NF, all values must be atomic (no repeating groups).

```sql
CREATE TABLE ClubMembership_1NF (
    StudentID INT,
    StudentName VARCHAR(50),
    Email VARCHAR(50),
    ClubName VARCHAR(50),
    ClubRoom VARCHAR(50),
    ClubMentor VARCHAR(50),
    JoinDate DATE,
    PRIMARY KEY (StudentID, ClubName, JoinDate)
);

SELECT * FROM ClubMembership_1NF;
```

###  Explanation

- Data is stored in one single table.
- Composite primary key: `(StudentID, ClubName, JoinDate)`
- Still contains redundancy.
- Student and club details repeat multiple times.

---

#  Step 2 – Second Normal Form (2NF)

To achieve 2NF:
- Remove partial dependencies.
- Separate student and club information into different tables.

## Create Student Table

```sql
CREATE TABLE Student (
    StudentID INT PRIMARY KEY,
    StudentName VARCHAR(50),
    Email VARCHAR(50)
);
```

## Create Club Table

```sql
CREATE TABLE Club (
    ClubID INT PRIMARY KEY,
    ClubName VARCHAR(50),
    ClubRoom VARCHAR(50),
    ClubMentor VARCHAR(50)
);
```

## Create Membership Table

```sql
CREATE TABLE Membership (
    MembershipID INT PRIMARY KEY AUTO_INCREMENT,
    StudentID INT,
    ClubID INT,
    JoinDate DATE,
    FOREIGN KEY (StudentID) REFERENCES Student(StudentID),
    FOREIGN KEY (ClubID) REFERENCES Club(ClubID)
);
```

## View Tables

```sql
SELECT * FROM Student;
SELECT * FROM Club;
SELECT * FROM Membership;
```

###  Explanation

- Student data stored separately.
- Club data stored separately.
- Membership table connects Student and Club.
- Foreign keys enforce referential integrity.
- Redundancy reduced significantly.

---

#  Step 3 – Third Normal Form (3NF)

To achieve 3NF:
- Remove transitive dependencies.
- Ensure non-key attributes depend only on the primary key.

```sql
CREATE TABLE StudentClub_3NF (
    StudentClubID INT PRIMARY KEY AUTO_INCREMENT,
    StudentID INT,
    ClubID INT,
    JoinDate DATE,
    FOREIGN KEY (StudentID) REFERENCES Student(StudentID),
    FOREIGN KEY (ClubID) REFERENCES Club(ClubID)
);
```


# 3.4 SQL JOIN Query

Retrieve **Student Name, Club Name, and Join Date**.

```sql
SELECT Student.StudentName, Club.ClubName, Membership.JoinDate
FROM Membership
JOIN Student ON Membership.StudentID = Student.StudentID
JOIN Club ON Membership.ClubID = Club.ClubID;
```

### Output

```
StudentName | ClubName       | JoinDate
-----------------------------------------
Mello       | Hacking Club   | 2025-05-10
```

---

# Conclusion

This project demonstrated how **encoding formats, computational complexity, and database normalization** support modern computing systems.

Encoding methods such as **Base64, URL encoding, and hexadecimal** allow safe transmission across protocols like **HTTPS, TLS, and REST APIs**. Computational complexity analysis shows how some problems become difficult as input size grows. Database normalization improves data integrity by organizing information into structured relational tables.

Together, these concepts support the development of **secure, efficient, and scalable information systems**.

---



