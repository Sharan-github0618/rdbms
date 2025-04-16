1. Retrieve the names of the instructors and order them by the departmental name.

sql
Copy code
SELECT name FROM INSTRUCTTOR ORDER BY deptname;
🔹 2. Retrieve instructors whose salary is between 10000–20000.

sql
Copy code
SELECT * FROM INSTRUCTTOR WHERE salary BETWEEN 10000 AND 20000;
🔹 3. Find the average salary of instructors for a particular department.

sql
Copy code
SELECT AVG(salary) FROM INSTRUCTTOR WHERE deptname = 'CSE';
🔹 4. Find the minimum salary of instructors for a particular department.

sql
Copy code
SELECT MIN(salary) FROM INSTRUCTTOR WHERE deptname = 'CSE';
🔹 5. Average salary department-wise where avg salary > 50000.

sql
Copy code
SELECT deptname, AVG(salary) AS avg_salary
FROM INSTRUCTTOR
GROUP BY deptname
HAVING AVG(salary) > 50000;
🔹 6. Display the name of instructors who teach in building 'Golden Jubilee Block'.

sql
Copy code
SELECT DISTINCT I.name
FROM INSTRUCTTOR I
JOIN TEACHES T ON I.iid = T.iid
JOIN SECTION S ON T.courseid = S.courseid AND T.secid = S.secid
WHERE S.building = 'Golden Jubilee Block';
🔹 7. Retrieve the total number of students in each department section-wise.

sql
Copy code
SELECT deptname, COUNT(stdid) AS total_students
FROM STUDENT
GROUP BY deptname;
🔹 8. Display name, salary from INSTRUCTOR of MBA whose salary is between 50000–70000.

sql
Copy code
SELECT name, salary FROM INSTRUCTTOR WHERE deptname = 'MBA' AND salary BETWEEN 50000 AND 70000;
🔹 9. Find names of students who took at least one MCA course.

sql
Copy code
SELECT DISTINCT S.name
FROM STUDENT S
JOIN TEXT T ON S.stdid = T.stdid
JOIN COURSE C ON T.courseid = C.courseid
WHERE C.deptname = 'MCA';
🔹 10. Find department name of all instructors.

sql
Copy code
SELECT DISTINCT deptname FROM INSTRUCTTOR;
🔹 11. Find enrollment of each section offered in 2015 and in ODD semesters.

sql
Copy code
SELECT courseid, secid, COUNT(stdid) AS enrollment
FROM TEXT
WHERE year = '2015' AND sem IN ('ODD')
GROUP BY courseid, secid;
🔹 12. Delete courses never offered (not in SECTION).

sql
Copy code
DELETE FROM COURSE
WHERE courseid NOT IN (SELECT DISTINCT courseid FROM SECTION);
🔹 13. Retrieve departments of a specific instructor (e.g., 'I101').

sql
Copy code
SELECT DISTINCT deptname FROM INSTRUCTTOR WHERE iid = 'I101';
🔹 14. Retrieve all course titles of a specific instructor.

sql
Copy code
SELECT DISTINCT C.title
FROM COURSE C
JOIN TEACHES T ON C.courseid = T.courseid
WHERE T.iid = 'I101';
🔹 15. Increase salary of each instructor by 25000 and list names and IDs.

sql
Copy code
UPDATE INSTRUCTTOR SET salary = salary + 25000;
SELECT iid, name FROM INSTRUCTTOR;
🔹 16. Retrieve instructors with their department and building.

sql
Copy code
SELECT I.name, I.deptname, D.building
FROM INSTRUCTTOR I
JOIN DEPARTMENT D ON I.deptname = D.deptname;
🔹 17. Instructors whose name includes 'ma' and courses they teach.

sql
Copy code
SELECT I.name, C.title
FROM INSTRUCTTOR I
JOIN TEACHES T ON I.iid = T.iid
JOIN COURSE C ON T.courseid = C.courseid
WHERE I.name LIKE '%ma%';
🔹 18. Instructors sorted by descending salary, with same name handling.

sql
Copy code
SELECT * FROM INSTRUCTTOR ORDER BY salary DESC, name;
🔹 19. Total credits offered by each department.

sql
Copy code
SELECT deptname, SUM(credits) AS total_credits
FROM COURSE
GROUP BY deptname;
🔹 20. Average salary of all instructors by department.

sql
Copy code
SELECT deptname, AVG(salary) AS avg_salary
FROM INSTRUCTTOR
GROUP BY deptname;
🔹 21. Courses taught in ODD 2018 but not in EVEN 2018.

sql
Copy code
SELECT courseid FROM SECTION
WHERE year = '2018' AND sem = 'ODD'
AND courseid NOT IN (
    SELECT courseid FROM SECTION WHERE year = '2018' AND sem = 'EVEN'
);
🔹 22. Instructors whose name is neither Charlie nor Deepika.

sql
Copy code
SELECT * FROM INSTRUCTTOR
WHERE name NOT IN ('Charlie', 'Deepika');
🔹 23. Create view for MCA course sections in EVEN 2016 with building and roomno.

sql
Copy code
CREATE VIEW mca_even2016 AS
SELECT S.courseid, S.secid, S.building, S.roomno
FROM SECTION S
JOIN COURSE C ON S.courseid = C.courseid
WHERE C.deptname = 'MCA' AND S.year = '2016' AND S.sem = 'EVEN';
🔹 24. Name of student(s) with max CGPA.

sql
Copy code
SELECT name FROM STUDENT
WHERE totalcredit = (SELECT MAX(totalcredit) FROM STUDENT);
🔹 25. MBA courses that run in roomno like '%MA' in 2015.

sql
Copy code
SELECT C.courseid
FROM COURSE C
JOIN SECTION S ON C.courseid = S.courseid
WHERE C.deptname = 'MBA' AND S.year = '2015' AND S.roomno LIKE '%MA';
🔹 26. Total CGPA (credits) scored by students of each department.

sql
Copy code
SELECT deptname, SUM(totalcredit) AS total_cgpa
FROM STUDENT
GROUP BY deptname;
🔹 27. Total number of credits offered by each department.

sql
Copy code
SELECT deptname, SUM(credits) AS total_credits
FROM COURSE
GROUP BY deptname;
🔹 28. Enrollment of each section offered in ODD 2019.

sql
Copy code
SELECT courseid, secid, COUNT(stdid) AS enrollment
FROM TEXT
WHERE year = '2019' AND sem = 'ODD'
GROUP BY courseid, secid;
🔹 29. Total number of instructors who teach in ODD 2019.

sql
Copy code
SELECT COUNT(DISTINCT iid) AS total_instructors
FROM TEACHES
WHERE sem = 'ODD' AND year = '2019';






2>-----------------------------------------
1. Find the names of suppliers who supply only red parts.
sql
Copy code
SELECT name
FROM SUPPLIER
WHERE supplier_id NOT IN (
    SELECT CATALOG.supplier_id
    FROM CATALOG
    JOIN PART ON CATALOG.part_id = PART.part_id
    WHERE color <> 'red'
)
AND supplier_id IN (
    SELECT CATALOG.supplier_id
    FROM CATALOG
    JOIN PART ON CATALOG.part_id = PART.part_id
    WHERE color = 'red'
);
🔹 2. Find supplierid of suppliers who supply red and green parts.
sql
Copy code
SELECT DISTINCT supplier_id
FROM CATALOG
WHERE supplier_id IN (
    SELECT supplier_id FROM CATALOG JOIN PART ON CATALOG.part_id = PART.part_id WHERE color = 'red'
)
AND supplier_id IN (
    SELECT supplier_id FROM CATALOG JOIN PART ON CATALOG.part_id = PART.part_id WHERE color = 'green'
);
🔹 3. Find the supplierid of supplier who supplies some red part OR whose address is 'Mysuru'.
sql
Copy code
SELECT DISTINCT supplier_id
FROM SUPPLIER
WHERE address = 'Mysuru'
OR supplier_id IN (
    SELECT supplier_id
    FROM CATALOG
    JOIN PART ON CATALOG.part_id = PART.part_id
    WHERE color = 'red'
);
🔹 4. Find the supplierid of suppliers who supply some red and some green parts.
sql
Copy code
SELECT supplier_id
FROM CATALOG
JOIN PART ON CATALOG.part_id = PART.part_id
WHERE color = 'red'
AND supplier_id IN (
    SELECT supplier_id
    FROM CATALOG
    JOIN PART ON CATALOG.part_id = PART.part_id
    WHERE color = 'green'
);
🔹 5. Find the supplierid of suppliers who supply every part.
sql
Copy code
SELECT supplier_id
FROM CATALOG
GROUP BY supplier_id
HAVING COUNT(DISTINCT part_id) = (SELECT COUNT(*) FROM PART);
