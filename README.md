
---

# ✅ **1. CREATE TABLE STATEMENTS**

(Exactly as your syllabus — safe to run)

```sql
CREATE TABLE STUDENT (
    RollNo CHAR(6) PRIMARY KEY,
    StudentName VARCHAR(20),
    Course VARCHAR(20),
    DOB DATE,
    Mobile CHAR(10) DEFAULT '9999999999'
);

CREATE TABLE SOCIETY (
    SocID CHAR(6) PRIMARY KEY,
    SocName VARCHAR(20),
    MentorName VARCHAR(20),
    TotalSeats INT UNSIGNED
);

CREATE TABLE ENROLLMENT (
    RollNo CHAR(6),
    SID CHAR(6),
    DateOfEnrollment DATE,
    FeesPaid ENUM('yes','no') DEFAULT 'no',
    FOREIGN KEY (RollNo) REFERENCES STUDENT(RollNo),
    FOREIGN KEY (SID) REFERENCES SOCIETY(SocID)
);
```

---

# ✅ **2. INSERT DATA**

## **STUDENT Table (15 students)**

(DOBs chosen to test age >20, youngest student, 2001 birth etc.)

```sql
INSERT INTO STUDENT (RollNo, StudentName, Course, DOB) VALUES
('A10001','Aarav','computer science','2004-05-10'),
('A10002','Ananya','chemistry','2003-11-22'),
('A10003','Rahul','physics','2001-06-15'),
('A10004','Zeeshan','computer science','2002-09-10'),
('A10005','Xavier','mathematics','1999-03-25'),
('A10006','Mehak','chemistry','2001-01-12'),
('A10007','Sohan','computer science','2004-12-18'),
('A10008','Priya','chemistry','2005-02-10'),
('A10009','Arjun','computer science','2000-04-17'),
('A10010','Isha','physics','2003-08-19'),
('A10011','Aditi','computer science','2004-09-10'),
('Z10019','Zara','biology','2002-11-11'),
('X20029','Xenia','mathematics','2001-04-04'),
('A30039','Aman','computer science','2005-01-30'),
('B10059','Bhoomi','chemistry','1998-12-02');
```

---

## **SOCIETY Table (6 societies)**

(Names include Debating, Dancing, NSS, Sashakt, etc.)

```sql
INSERT INTO SOCIETY (SocID, SocName, MentorName, TotalSeats) VALUES
('s1','Debating','Rohan Gupta',30),
('s2','Dancing','Meera Singh',20),
('s3','NSS','Arvind Kumar',50),
('s4','Sashakt','Priya Gupta',25),
('s5','Drama','S. Bose',15),
('s6','Coding','Harish Mehta',40);
```

---

## **ENROLLMENT Table**

Dataset ensures:

* Students in multiple societies
* Some societies heavily filled (s1, s3)
* Some barely filled (s5)
* Some unenrolled students exist
* One student in **all societies** for Question 24

```sql
INSERT INTO ENROLLMENT (RollNo, SID, DateOfEnrollment, FeesPaid) VALUES
('A10001','s1','2023-01-10','yes'),
('A10001','s3','2023-02-15','no'),

('A10002','s1','2023-03-12','yes'),
('A10002','s2','2023-04-01','no'),

('A10003','s3','2023-01-18','yes'),

('A10004','s2','2023-03-22','no'),
('A10004','s4','2023-04-11','yes'),

('A10005','s1','2023-02-10','yes'),
('A10005','s6','2023-03-15','no'),

('A10006','s3','2023-03-29','yes'),
('A10006','s5','2023-03-29','yes'),

('A10007','s1','2023-01-11','yes'),

('A10008','s4','2023-02-14','no'),

('A10009','s3','2023-04-20','yes'),
('A10009','s1','2023-05-05','yes'),

('A10010','s6','2023-01-19','yes'),

('A10011','s5','2023-03-28','no'),

('Z10019','s6','2023-02-05','yes'),

('X20029','s1','2023-01-12','yes'),
('X20029','s2','2023-01-13','yes'),
('X20029','s3','2023-01-14','yes'),
('X20029','s4','2023-01-15','yes'),
('X20029','s5','2023-01-16','yes'),
('X20029','s6','2023-01-17','yes');  -- enrolled in ALL societies

('A30039','s2','2023-03-01','no');
```

---

# ✅ **3. UNENROLLED STUDENTS (for Q12)**

These students are intentionally NOT enrolled:

* **Isha — A10010** → enrolled
* **Bhoomi — B10059** → NOT enrolled

So Q12 works perfectly.

---


# ✅ **1. Retrieve names of students enrolled in any society**

```sql
SELECT DISTINCT s.StudentName
FROM STUDENT s
JOIN ENROLLMENT e ON s.RollNo = e.RollNo;
```

---

# ✅ **2. Retrieve all society names**

```sql
SELECT SocName FROM SOCIETY;
```

---

# ✅ **3. Retrieve students' names starting with the letter ‘A’**

```sql
SELECT StudentName
FROM STUDENT
WHERE StudentName LIKE 'A%';
```

---

# ✅ **4. Retrieve students studying in ‘computer science’ or ‘chemistry’**

```sql
SELECT *
FROM STUDENT
WHERE Course IN ('computer science', 'chemistry');
```

---

# ✅ **5. Students whose roll no starts with ‘X’ or ‘Z’ and ends with ‘9’**

```sql
SELECT StudentName
FROM STUDENT
WHERE (RollNo LIKE 'X%9' OR RollNo LIKE 'Z%9');
```

---

# ✅ **6. Societies with more than N TotalSeats (N given by user)**

```sql
SELECT *
FROM SOCIETY
WHERE TotalSeats > N;
```

---

# ✅ **7. Update mentor name of a specific society**

```sql
UPDATE SOCIETY
SET MentorName = 'NewMentor'
WHERE SocID = 's1';
```

---

# ✅ **8. Societies with more than five students enrolled**

```sql
SELECT s.SocName
FROM SOCIETY s
JOIN ENROLLMENT e ON s.SocID = e.SID
GROUP BY s.SocID
HAVING COUNT(e.RollNo) > 5;
```

---

# ✅ **9. Youngest student enrolled in society ‘NSS’**

```sql
SELECT st.StudentName
FROM STUDENT st
JOIN ENROLLMENT e ON st.RollNo = e.RollNo
JOIN SOCIETY so ON so.SocID = e.SID
WHERE so.SocName = 'NSS'
ORDER BY st.DOB DESC
LIMIT 1;
```

---

# ✅ **10. Most popular society (highest number of enrollments)**

```sql
SELECT s.SocName
FROM SOCIETY s
JOIN ENROLLMENT e ON s.SocID = e.SID
GROUP BY s.SocID
ORDER BY COUNT(e.RollNo) DESC
LIMIT 1;
```

---

# ✅ **11. Two least popular societies**

```sql
SELECT s.SocName
FROM SOCIETY s
LEFT JOIN ENROLLMENT e ON s.SocID = e.SID
GROUP BY s.SocID
ORDER BY COUNT(e.RollNo)
LIMIT 2;
```

---

# ✅ **12. Students not enrolled in any society**

```sql
SELECT s.StudentName
FROM STUDENT s
LEFT JOIN ENROLLMENT e ON s.RollNo = e.RollNo
WHERE e.RollNo IS NULL;
```

---

# ✅ **13. Students enrolled in at least two societies**

```sql
SELECT st.StudentName
FROM STUDENT st
JOIN ENROLLMENT e ON st.RollNo = e.RollNo
GROUP BY st.RollNo
HAVING COUNT(e.SID) >= 2;
```

---

# ✅ **14. Society names in which maximum students are enrolled**

```sql
SELECT s.SocName
FROM SOCIETY s
JOIN ENROLLMENT e ON s.SocID = e.SID
GROUP BY s.SocID
HAVING COUNT(e.RollNo) =
(
    SELECT MAX(cnt)
    FROM (
        SELECT COUNT(*) AS cnt
        FROM ENROLLMENT
        GROUP BY SID
    ) t
);
```

---

# ✅ **15. Students + society names where at least one student is enrolled**

```sql
SELECT st.StudentName, so.SocName
FROM STUDENT st
JOIN ENROLLMENT e ON st.RollNo = e.RollNo
JOIN SOCIETY so ON so.SocID = e.SID;
```

---

# ✅ **16. Students enrolled in Debating, Dancing, or Sashakt**

```sql
SELECT DISTINCT st.StudentName
FROM STUDENT st
JOIN ENROLLMENT e ON st.RollNo = e.RollNo
JOIN SOCIETY so ON so.SocID = e.SID
WHERE so.SocName IN ('Debating', 'Dancing', 'Sashakt');
```

---

# ✅ **17. Society names whose mentor's name contains ‘Gupta’**

```sql
SELECT SocName
FROM SOCIETY
WHERE MentorName LIKE '%Gupta%';
```

---

# ✅ **18. Societies where enrolled students = 10% of its capacity**

```sql
SELECT s.SocName
FROM SOCIETY s
LEFT JOIN ENROLLMENT e ON s.SocID = e.SID
GROUP BY s.SocID
HAVING COUNT(e.RollNo) = 0.1 * s.TotalSeats;
```

---

# ✅ **19. Display vacant seats for each society**

```sql
SELECT s.SocName,
       s.TotalSeats - COUNT(e.RollNo) AS VacantSeats
FROM SOCIETY s
LEFT JOIN ENROLLMENT e ON s.SocID = e.SID
GROUP BY s.SocID;
```

---

# ✅ **20. Increment total seats of each society by 10%**

```sql
UPDATE SOCIETY
SET TotalSeats = TotalSeats * 1.10;
```

---

# ✅ **21. Add enrollment fees column (‘yes’/’no’)**

```sql
ALTER TABLE ENROLLMENT
ADD FeesPaid ENUM('yes','no') DEFAULT 'no';
```

---

# ✅ **22. Update enrollment dates for s1, s2, s3**

```sql
UPDATE ENROLLMENT
SET DateOfEnrollment = '2018-01-15'
WHERE SID = 's1';

UPDATE ENROLLMENT
SET DateOfEnrollment = CURRENT_DATE()
WHERE SID = 's2';

UPDATE ENROLLMENT
SET DateOfEnrollment = '2018-01-02'
WHERE SID = 's3';
```

---

# ✅ **23. Create a view: Society + total enrolled**

```sql
CREATE VIEW SocietyEnrollmentCount AS
SELECT s.SocID, s.SocName, COUNT(e.RollNo) AS TotalEnrolled
FROM SOCIETY s
LEFT JOIN ENROLLMENT e ON s.SocID = e.SID
GROUP BY s.SocID;
```

---

# ✅ **24. Students enrolled in ALL societies**

```sql
SELECT st.StudentName
FROM STUDENT st
JOIN ENROLLMENT e ON st.RollNo = e.RollNo
GROUP BY st.RollNo
HAVING COUNT(DISTINCT e.SID) = (SELECT COUNT(*) FROM SOCIETY);
```

---

# ✅ **25. Count societies with more than 5 students enrolled**

```sql
SELECT COUNT(*)
FROM (
    SELECT SID
    FROM ENROLLMENT
    GROUP BY SID
    HAVING COUNT(RollNo) > 5
) t;
```

---

# ✅ **26. Add mobile number field in student table**

```sql
ALTER TABLE STUDENT
ADD Mobile CHAR(10) DEFAULT '9999999999';
```

---

# ✅ **27. Count students age > 20**

```sql
SELECT COUNT(*)
FROM STUDENT
WHERE TIMESTAMPDIFF(YEAR, DOB, CURDATE()) > 20;
```

---

# ✅ **28. Students born in 2001 enrolled in at least one society**

```sql
SELECT DISTINCT st.StudentName
FROM STUDENT st
JOIN ENROLLMENT e ON st.RollNo = e.RollNo
WHERE YEAR(st.DOB) = 2001;
```

---

# ✅ **29. Count societies whose name starts with ‘S’, ends with ‘t’, and have ≥5 students**

```sql
SELECT COUNT(*)
FROM SOCIETY s
JOIN ENROLLMENT e ON s.SocID = e.SID
WHERE s.SocName LIKE 'S%t'
GROUP BY s.SocID
HAVING COUNT(e.RollNo) >= 5;
```

---

# ✅ **30. Display: Society name, Mentor name, Total Capacity, Total Enrolled, Unfilled Seats**

```sql
SELECT s.SocName,
       s.MentorName,
       s.TotalSeats AS TotalCapacity,
       COUNT(e.RollNo) AS TotalEnrolled,
       (s.TotalSeats - COUNT(e.RollNo)) AS UnfilledSeats
FROM SOCIETY s
LEFT JOIN ENROLLMENT e ON s.SocID = e.SID
GROUP BY s.SocID;
```

---

.
