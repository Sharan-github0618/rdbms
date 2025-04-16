# rdbms
✅ 1. Names of instructors ordered by department name:
sql
Copy code
SELECT name
FROM INSTRUCTTOR
ORDER BY deptname;
✅ 2. Instructors with salary between 10000 and 20000:
sql
Copy code
SELECT name, salary
FROM INSTRUCTTOR
WHERE salary BETWEEN 10000 AND 20000;
✅ 3. Average salary of instructors in a specific department:
sql
Copy code
SELECT AVG(salary) AS avg_salary
FROM INSTRUCTTOR
WHERE deptname = 'YourDepartment';
✅ 4. Minimum salary of instructors in a specific department:
sql
Copy code
SELECT MIN(salary) AS min_salary
FROM INSTRUCTTOR
WHERE deptname = 'YourDepartment';
✅ 5. Average salary grouped by department where avg > 50000:
sql
Copy code
SELECT deptname, AVG(salary) AS avg_salary
FROM INSTRUCTTOR
GROUP BY deptname
HAVING AVG(salary) > 50000;
✅ 6. Instructors teaching in 'GOLDEN JUBLEE BLOCK':
sql
Copy code
SELECT DISTINCT I.name
FROM INSTRUCTTOR I
JOIN TEACHES T ON I.iid = T.iid
JOIN SECTION S ON T.courseid = S.courseid AND T.secid = S.secid
WHERE S.building = 'GOLDEN JUBLEE BLOCK';
✅ 7. Total number of students in each department section-wise:
sql
Copy code
SELECT deptname, COUNT(*) AS total_students
FROM STUDENT
GROUP BY deptname;
✅ 8. Name, Salary from INSTRUCTOR of MBA department with salary between 50000-70000:
sql
Copy code
SELECT name, salary
FROM INSTRUCTTOR
WHERE deptname = 'MBA' AND salary BETWEEN 50000 AND 70000;
✅ 9. Names of all students who took at least one MCA course:
sql
Copy code
SELECT DISTINCT S.name
FROM STUDENT S
JOIN TEXT T ON S.stdid = T.stdid
JOIN COURSE C ON T.courseid = C.courseid
WHERE C.deptname = 'MCA';
✅ 10. Department names of all instructors:
sql
Copy code
SELECT DISTINCT deptname
FROM INSTRUCTTOR;
✅ 11. Enrollment of each section offered in 2015 and odd semesters:
sql
Copy code
SELECT courseid, secid, COUNT(*) AS enrollment
FROM TEXT
WHERE year = '2015' AND (sem = 'ODD' OR sem = 'Odd')
GROUP BY courseid, secid;
✅ 12. Delete courses never offered (not in SECTION):
sql
Copy code
DELETE FROM COURSE
WHERE courseid NOT IN (SELECT DISTINCT courseid FROM SECTION);
✅ 13. Departments of a specific instructor:
sql
Copy code
SELECT DISTINCT deptname
FROM INSTRUCTTOR
WHERE name = 'Specific Instructor Name';
✅ 14. All course titles of a specific instructor:
sql
Copy code
SELECT DISTINCT C.title
FROM COURSE C
JOIN TEACHES T ON C.courseid = T.courseid
WHERE T.iid = 'InstructorID';
✅ 15. Increase salary by 25000 and list names & ids:
sql
Copy code
UPDATE INSTRUCTTOR
SET salary = salary + 25000;

SELECT iid, name
FROM INSTRUCTTOR;
✅ 16. Instructors with dept name and building name:
sql
Copy code
SELECT I.name, I.deptname, D.building
FROM INSTRUCTTOR I
JOIN DEPARTMENT D ON I.deptname = D.deptname;
✅ 17. Instructors with names containing 'MA' and their courses:
sql
Copy code
SELECT I.name, C.title
FROM INSTRUCTTOR I
JOIN TEACHES T ON I.iid = T.iid
JOIN COURSE C ON T.courseid = C.courseid
WHERE I.name LIKE '%MA%';
✅ 18. Instructors ordered by salary (desc) even if names are same:
sql
Copy code
SELECT *
FROM INSTRUCTTOR
ORDER BY salary DESC, name;
✅ 19. Total number of credits offered by each department:
sql
Copy code
SELECT deptname, SUM(credits) AS total_credits
FROM COURSE
GROUP BY deptname;
✅ 20. Average salary of all instructors by department:
sql
Copy code
SELECT deptname, AVG(salary) AS avg_salary
FROM INSTRUCTTOR
GROUP BY deptname;
✅ 21. Courses taught in ODD 2018 but not in EVEN 2018:
sql
Copy code
SELECT courseid
FROM SECTION
WHERE year = '2018' AND sem = 'ODD'
EXCEPT
SELECT courseid
FROM SECTION
WHERE year = '2018' AND sem = 'EVEN';
✅ 22. Instructor names not Charlie or Deepika:
sql
Copy code
SELECT name
FROM INSTRUCTTOR
WHERE name NOT IN ('Charlie', 'Deepika');
✅ 23. View for MCA course sections in EVEN 2016 with room and building:
sql
Copy code
CREATE VIEW mca_sections_even2016 AS
SELECT S.courseid, S.secid, S.building, S.roomno
FROM SECTION S
JOIN COURSE C ON S.courseid = C.courseid
WHERE C.deptname = 'MCA' AND S.sem = 'EVEN' AND S.year = '2016';
✅ 24. Student(s) with max CGPA (assuming grade is CGPA):
sql
Copy code
SELECT name
FROM STUDENT
WHERE totalcredit = (SELECT MAX(totalcredit) FROM STUDENT);
✅ 25. MBA course in MBAIS room during 2015 like '%MA':
sql
Copy code
SELECT C.title
FROM COURSE C
JOIN SECTION S ON C.courseid = S.courseid
WHERE C.deptname = 'MBA' AND S.roomno LIKE '%MBAIS%' AND S.year = '2015' AND C.title LIKE '%MA%';
✅ 26. Total CGPA (assuming totalcredit means CGPA) by department:
sql
Copy code
SELECT deptname, SUM(totalcredit) AS total_cgpa
FROM STUDENT
GROUP BY deptname;
✅ 27. Total credits offered by each department (repeated):
(Refer to query 19)

✅ 28. Enrollment of each section in ODD 2019:
sql
Copy code
SELECT courseid, secid, COUNT(*) AS enrollment
FROM TEXT
WHERE year = '2019' AND sem = 'ODD'
GROUP BY courseid, secid;
✅ 29. Number of instructors teaching in ODD 2019:
sql
Copy code
SELECT COUNT(DISTINCT iid) AS total_instructors
FROM TEACHES
WHERE year = '2019' AND sem = 'ODD';






2>-----------------------------------------
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
✅ b) Supplier IDs who supply red and green parts
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
✅ c) Supplier IDs who supply red part OR whose address is 'Mysuru':
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
✅ d) Supplier IDs who supply some red and some green parts
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
✅ e) Supplier IDs who supply every part
sql
Copy code
SELECT supplier_id
FROM CATALOG
GROUP BY supplier_id
HAVING COUNT(DISTINCT part_id) = (SELECT COUNT(*) FROM PART);
