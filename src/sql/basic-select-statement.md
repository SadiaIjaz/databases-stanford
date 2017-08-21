# SQL - Basic SELECT Statement
> :dvd: Annotations from the course [SQL](https://lagunita.stanford.edu/courses/DB/SQL/SelfPaced/info) - Stanford

## Basic SELECT Statement
> Some tips to setup your database [here](https://ericdouglas.github.io/2017/08/15/basic-cli-postgres-management/).

```sql
SELECT A1, A2, ..., An # What to return
FROM R1, R2, ..., Rm # relations
WHERE condition # combine | filter | etc
```

**Create table college**
```sql
CREATE TABLE college (
  cname varchar (128) PRIMARY KEY,
  state varchar (2) check (state in ('CA', 'NY', 'MA')),
  enrollment int
);
```

**Populate table college**
```sql
INSERT INTO college (cname, state, enrollment) VALUES
  ('Stanford', 'CA', 15000),
  ('Berkeley', 'CA', 36000),
  ('MIT', 'MA', 10000),
  ('Cornell', 'NY', 21000);
```

**Create table student**
```sql
CREATE TABLE student (
  sid serial PRIMARY KEY,
  sname varchar (128),
  gpa float (1),
  size_hs int
);
```

**Populate table student**
```sql
INSERT INTO student (sid, sname, gpa, size_hs) VALUES
  (123, 'Amy', 3.9, 1000),
  (234, 'Bob', 3.6, 1500),
  (345, 'Craig', 3.5, 500),
  (456, 'Doris', 3.9, 1000),
  (567, 'Edward', 2.9, 2000),
  (678, 'Fay', 3.8, 200),
  (789, 'Gary', 3.4, 800),
  (987, 'Helen', 3.7, 800),
  (876, 'Irene', 3.9, 400),
  (765, 'Jay', 2.9, 1500),
  (654, 'Amy', 3.9, 1000),
  (543, 'Craig', 3.4, 2000);
```

**Create table apply**
```sql
CREATE TABLE apply (
  sid serial,
  cname varchar (128),
  major varchar (64),
  decision char CHECK (decision in ('y', 'n'))
);
```

**Populate table apply**
```sql
INSERT INTO apply (sid, cname, major, decision) VALUES
  (123, 'Stanford', 'CS', 'y'),
  (123, 'Stanford', 'EE', 'n'),
  (123, 'Berkeley', 'CS', 'y'),
  (123, 'Cornell', 'EE', 'y'),
  (234, 'Berkeley', 'biology', 'n'),
  (345, 'MIT', 'bioengineering', 'y'),
  (345, 'Cornell', 'bioengineering', 'n'),
  (345, 'Cornell', 'CS', 'y'),
  (345, 'Cornell', 'EE', 'n'),
  (678, 'Stanford', 'history', 'y'),
  (987, 'Stanford', 'CS', 'y'),
  (987, 'Berkeley', 'CS', 'y'),
  (876, 'Stanford', 'CS', 'n'),
  (876, 'MIT', 'biology', 'y'),
  (876, 'MIT', 'marine biology', 'n'),
  (765, 'Stanford', 'history', 'y'),
  (765, 'Cornell', 'hisotry', 'n'),
  (765, 'Cornell', 'psychology', 'y'),
  (543, 'MIT', 'CS', 'n');
```

First query:

```sql
SELECT sid, sname, gpa
FROM student
WHERE gpa > 3.6;
```

Result:
```
 sid | sname | gpa
-----+-------+-----
 123 | Amy   | 3.9
 456 | Doris | 3.9
 678 | Fay   | 3.8
 987 | Helen | 3.7
 876 | Irene | 3.9
 654 | Amy   | 3.9
(6 rows)
```

Second query combine two relations. Find the name of students and the major they applied.
```sql
SELECT sname, major
FROM student, apply
WHERE student.sid = apply.sid;
```

Result:
```
 sname |     major
-------+----------------
 Amy   | EE
 Amy   | CS
 Amy   | EE
 Amy   | CS
 Bob   | biology
 Craig | EE
 Craig | CS
 Craig | bioengineering
 Craig | bioengineering
 Fay   | history
 Helen | CS
 Helen | CS
 Irene | marine biology
 Irene | biology
 Irene | CS
 Jay   | psychology
 Jay   | hisotry
 Jay   | history
 Craig | CS
(19 rows)
```

To eliminate duplicate results, add the `DISTINCT` keyword
```sql
SELECT DISTINCT sname, major
FROM student, apply
WHERE student.sid = apply.sid;
```

Result:
```
 sname |     major
-------+----------------
 Irene | marine biology
 Fay   | history
 Irene | CS
 Craig | EE
 Irene | biology
 Craig | bioengineering
 Craig | CS
 Jay   | hisotry
 Amy   | EE
 Jay   | psychology
 Helen | CS
 Jay   | history
 Bob   | biology
 Amy   | CS
(14 rows)
```

Next query: find the names, GPAs and decision of students whose size high school is less than a thousand and they'he applied to CS at Stanford.
```sql
SELECT sname, GPA, decision
FROM student, apply
WHERE student.sid = apply.sid
AND size_hs < 1000
AND major = 'CS'
AND cname = 'Stanford';
```

Result:
```
 sname | gpa | decision
-------+-----+----------
 Helen | 3.7 | y
 Irene | 3.9 | n
(2 rows)
```

Next query: find all large campuses that have someone applying to that campus in CS. This time we're goind to join the college table and the apply table.

> **OBS:** when we have two columns with the same name in different tables, we need to prefix the select clause with the name of the table.

```sql
SELECT college.cname
FROM college, apply
WHERE college.cname = apply.cname
AND enrollment > 20000
AND major = 'CS';
```

Result:
```
  cname
----------
 Berkeley
 Cornell
 Berkeley
(3 rows)
```

Next query: Let's join all three relations. We're going to apply join conditions that ensure that we're talking about the same student and the same college. The result of that big cross-product, that big join, we're going to get the student ID, their name, their GPA, the college they're applying to and the enrollment of that college.

```sql
SELECT student.sid, sname, gpa, apply.cname, enrollment
FROM student, college, apply
WHERE apply.sid = student.sid
AND apply.cname = college.cname;
```

Result:
```
 sid | sname | gpa |  cname   | enrollment
-----+-------+-----+----------+------------
 123 | Amy   | 3.9 | Cornell  |      21000
 123 | Amy   | 3.9 | Berkeley |      36000
 123 | Amy   | 3.9 | Stanford |      15000
 123 | Amy   | 3.9 | Stanford |      15000
 234 | Bob   | 3.6 | Berkeley |      36000
 345 | Craig | 3.5 | Cornell  |      21000
 345 | Craig | 3.5 | Cornell  |      21000
 345 | Craig | 3.5 | Cornell  |      21000
 345 | Craig | 3.5 | MIT      |      10000
 678 | Fay   | 3.8 | Stanford |      15000
 987 | Helen | 3.7 | Berkeley |      36000
 987 | Helen | 3.7 | Stanford |      15000
 876 | Irene | 3.9 | MIT      |      10000
 876 | Irene | 3.9 | MIT      |      10000
 876 | Irene | 3.9 | Stanford |      15000
 765 | Jay   | 2.9 | Cornell  |      21000
 765 | Jay   | 2.9 | Cornell  |      21000
 765 | Jay   | 2.9 | Stanford |      15000
 543 | Craig | 3.4 | MIT      |      10000
(19 rows)
```

Next query: if we care about the order of our result, SQL provides a clause that we can ask for a result to be sorted by a particular attribute - `ORDER BY`. Let's add another clause to the previous query saying we want our application information sorted by descending GPA.

```sql
SELECT student.sid, sname, gpa, apply.cname, enrollment
FROM student, college, apply
WHERE apply.sid = student.sid
AND apply.cname = college.cname
ORDER BY gpa DESC;
```

Result:
```
 sid | sname | gpa |  cname   | enrollment
-----+-------+-----+----------+------------
 123 | Amy   | 3.9 | Cornell  |      21000
 123 | Amy   | 3.9 | Berkeley |      36000
 123 | Amy   | 3.9 | Stanford |      15000
 123 | Amy   | 3.9 | Stanford |      15000
 876 | Irene | 3.9 | Stanford |      15000
 876 | Irene | 3.9 | MIT      |      10000
 876 | Irene | 3.9 | MIT      |      10000
 678 | Fay   | 3.8 | Stanford |      15000
 987 | Helen | 3.7 | Berkeley |      36000
 987 | Helen | 3.7 | Stanford |      15000
 234 | Bob   | 3.6 | Berkeley |      36000
 345 | Craig | 3.5 | Cornell  |      21000
 345 | Craig | 3.5 | MIT      |      10000
 345 | Craig | 3.5 | Cornell  |      21000
 345 | Craig | 3.5 | Cornell  |      21000
 543 | Craig | 3.4 | MIT      |      10000
 765 | Jay   | 2.9 | Cornell  |      21000
 765 | Jay   | 2.9 | Cornell  |      21000
 765 | Jay   | 2.9 | Stanford |      15000
(19 rows)
```

Next query: adding another clause to the previous query - ascending enrollment.

```sql
SELECT student.sid, sname, gpa, apply.cname, enrollment
FROM student, college, apply
WHERE apply.sid = student.sid
AND apply.cname = college.cname;
ORDER BY gpa DESC, enrollment;
```

Result:
```
 sid | sname | gpa |  cname   | enrollment
-----+-------+-----+----------+------------
 876 | Irene | 3.9 | MIT      |      10000
 876 | Irene | 3.9 | MIT      |      10000
 876 | Irene | 3.9 | Stanford |      15000
 123 | Amy   | 3.9 | Stanford |      15000
 123 | Amy   | 3.9 | Stanford |      15000
 123 | Amy   | 3.9 | Cornell  |      21000
 123 | Amy   | 3.9 | Berkeley |      36000
 678 | Fay   | 3.8 | Stanford |      15000
 987 | Helen | 3.7 | Stanford |      15000
 987 | Helen | 3.7 | Berkeley |      36000
 234 | Bob   | 3.6 | Berkeley |      36000
 345 | Craig | 3.5 | MIT      |      10000
 345 | Craig | 3.5 | Cornell  |      21000
 345 | Craig | 3.5 | Cornell  |      21000
 345 | Craig | 3.5 | Cornell  |      21000
 543 | Craig | 3.4 | MIT      |      10000
 765 | Jay   | 2.9 | Stanford |      15000
 765 | Jay   | 2.9 | Cornell  |      21000
 765 | Jay   | 2.9 | Cornell  |      21000
(19 rows)
```

Next query: Introduces the `like` predicate. It is a built-in operator in SQL that allows us to do simple string matching on attribute values. Let's find all applications related to the "bio" type and project the `sid` and `major` columns.

```sql
SELECT sid, major
FROM apply
WHERE major
LIKE '%bio%';
```

Result:
```
 sid |     major
-----+----------------
 234 | biology
 345 | bioengineering
 345 | bioengineering
 876 | biology
 876 | marine biology
(5 rows)
```

Next query: Use the construct `SELECT *` to get all attributes in the result of the `FROM` and `WHERE` expression.

```sql
SELECT *
FROM apply
WHERE major
LIKE '%bio%';
```

Result:
```
 sid |  cname   |     major      | decision
-----+----------+----------------+----------
 234 | Berkeley | biology        | n
 345 | MIT      | bioengineering | y
 345 | Cornell  | bioengineering | n
 876 | MIT      | biology        | y
 876 | MIT      | marine biology | n
(5 rows)
```

Next query: gigantic query. A cross-product between student and college table without any combination.

```sql
SELECT *
FROM student, college;
```

Result:

```
 sid | sname  | gpa | size_hs |  cname   | state | enrollment
-----+--------+-----+---------+----------+-------+------------
 123 | Amy    | 3.9 |    1000 | Stanford | CA    |      15000
 123 | Amy    | 3.9 |    1000 | Berkeley | CA    |      36000
 123 | Amy    | 3.9 |    1000 | MIT      | MA    |      10000
 123 | Amy    | 3.9 |    1000 | Cornell  | NY    |      21000
 234 | Bob    | 3.6 |    1500 | Stanford | CA    |      15000
 234 | Bob    | 3.6 |    1500 | Berkeley | CA    |      36000
 234 | Bob    | 3.6 |    1500 | MIT      | MA    |      10000
 234 | Bob    | 3.6 |    1500 | Cornell  | NY    |      21000
 345 | Craig  | 3.5 |     500 | Stanford | CA    |      15000
 345 | Craig  | 3.5 |     500 | Berkeley | CA    |      36000
 345 | Craig  | 3.5 |     500 | MIT      | MA    |      10000
 345 | Craig  | 3.5 |     500 | Cornell  | NY    |      21000
 456 | Doris  | 3.9 |    1000 | Stanford | CA    |      15000
 456 | Doris  | 3.9 |    1000 | Berkeley | CA    |      36000
 456 | Doris  | 3.9 |    1000 | MIT      | MA    |      10000
 456 | Doris  | 3.9 |    1000 | Cornell  | NY    |      21000
 567 | Edward | 2.9 |    2000 | Stanford | CA    |      15000
 567 | Edward | 2.9 |    2000 | Berkeley | CA    |      36000
 567 | Edward | 2.9 |    2000 | MIT      | MA    |      10000
 567 | Edward | 2.9 |    2000 | Cornell  | NY    |      21000
 678 | Fay    | 3.8 |     200 | Stanford | CA    |      15000
 678 | Fay    | 3.8 |     200 | Berkeley | CA    |      36000
 678 | Fay    | 3.8 |     200 | MIT      | MA    |      10000
 678 | Fay    | 3.8 |     200 | Cornell  | NY    |      21000
 789 | Gary   | 3.4 |     800 | Stanford | CA    |      15000
 789 | Gary   | 3.4 |     800 | Berkeley | CA    |      36000
 789 | Gary   | 3.4 |     800 | MIT      | MA    |      10000
 789 | Gary   | 3.4 |     800 | Cornell  | NY    |      21000
 987 | Helen  | 3.7 |     800 | Stanford | CA    |      15000
 987 | Helen  | 3.7 |     800 | Berkeley | CA    |      36000
 987 | Helen  | 3.7 |     800 | MIT      | MA    |      10000
 987 | Helen  | 3.7 |     800 | Cornell  | NY    |      21000
 876 | Irene  | 3.9 |     400 | Stanford | CA    |      15000
 876 | Irene  | 3.9 |     400 | Berkeley | CA    |      36000
 876 | Irene  | 3.9 |     400 | MIT      | MA    |      10000
 876 | Irene  | 3.9 |     400 | Cornell  | NY    |      21000
 765 | Jay    | 2.9 |    1500 | Stanford | CA    |      15000
 765 | Jay    | 2.9 |    1500 | Berkeley | CA    |      36000
 765 | Jay    | 2.9 |    1500 | MIT      | MA    |      10000
 765 | Jay    | 2.9 |    1500 | Cornell  | NY    |      21000
 654 | Amy    | 3.9 |    1000 | Stanford | CA    |      15000
 654 | Amy    | 3.9 |    1000 | Berkeley | CA    |      36000
 654 | Amy    | 3.9 |    1000 | MIT      | MA    |      10000
 654 | Amy    | 3.9 |    1000 | Cornell  | NY    |      21000
 543 | Craig  | 3.4 |    2000 | Stanford | CA    |      15000
 543 | Craig  | 3.4 |    2000 | Berkeley | CA    |      36000
 543 | Craig  | 3.4 |    2000 | MIT      | MA    |      10000
 543 | Craig  | 3.4 |    2000 | Cornell  | NY    |      21000
(48 rows)
```

Next query: Demonstrate the ability to use arithmetic within SQL clauses. We'll create a query that selects all the information from the student relation but adds to it a scaled GPA where we're going to boost the student's GPA if they're from a big high school and reduce it if they're from a small one.

```sql
SELECT sid, sname, gpa, size_hs, gpa * (size_hs / 1000.0)
FROM student;
```

Result:

```
 sid | sname  | gpa | size_hs |     ?column?
-----+--------+-----+---------+-------------------
 123 | Amy    | 3.9 |    1000 |  3.90000009536743
 234 | Bob    | 3.6 |    1500 |  5.39999985694885
 345 | Craig  | 3.5 |     500 |              1.75
 456 | Doris  | 3.9 |    1000 |  3.90000009536743
 567 | Edward | 2.9 |    2000 |  5.80000019073486
 678 | Fay    | 3.8 |     200 | 0.759999990463257
 789 | Gary   | 3.4 |     800 |  2.72000007629395
 987 | Helen  | 3.7 |     800 |  2.96000003814697
 876 | Irene  | 3.9 |     400 |  1.56000003814697
 765 | Jay    | 2.9 |    1500 |  4.35000014305115
 654 | Amy    | 3.9 |    1000 |  3.90000009536743
 543 | Craig  | 3.4 |    2000 |  6.80000019073486
(12 rows)
```

Next query: Demonstrate the `as` feature which allows us to change the labeling of the schema in a query result.

```sql
SELECT sid, sname, gpa, size_hs, gpa * (size_hs / 1000.0) as scaled_gpa
FROM student;
```

Result:
```
 sid | sname  | gpa | size_hs |    scaled_gpa
-----+--------+-----+---------+-------------------
 123 | Amy    | 3.9 |    1000 |  3.90000009536743
 234 | Bob    | 3.6 |    1500 |  5.39999985694885
 345 | Craig  | 3.5 |     500 |              1.75
 456 | Doris  | 3.9 |    1000 |  3.90000009536743
 567 | Edward | 2.9 |    2000 |  5.80000019073486
 678 | Fay    | 3.8 |     200 | 0.759999990463257
 789 | Gary   | 3.4 |     800 |  2.72000007629395
 987 | Helen  | 3.7 |     800 |  2.96000003814697
 876 | Irene  | 3.9 |     400 |  1.56000003814697
 765 | Jay    | 2.9 |    1500 |  4.35000014305115
 654 | Amy    | 3.9 |    1000 |  3.90000009536743
 543 | Craig  | 3.4 |    2000 |  6.80000019073486
(12 rows)
```

**Additional**: format the scaled_gpa to show 2 decimal places.

```sql
SELECT sid,
       sname,
       gpa,
       size_hs,
       ROUND(CAST(gpa * (size_hs / 1000.0) as numeric), 2) as scaled_gpa
FROM student;
```

Result:

```
 sid | sname  | gpa | size_hs | scaled_gpa
-----+--------+-----+---------+------------
 123 | Amy    | 3.9 |    1000 |       3.90
 234 | Bob    | 3.6 |    1500 |       5.40
 345 | Craig  | 3.5 |     500 |       1.75
 456 | Doris  | 3.9 |    1000 |       3.90
 567 | Edward | 2.9 |    2000 |       5.80
 678 | Fay    | 3.8 |     200 |       0.76
 789 | Gary   | 3.4 |     800 |       2.72
 987 | Helen  | 3.7 |     800 |       2.96
 876 | Irene  | 3.9 |     400 |       1.56
 765 | Jay    | 2.9 |    1500 |       4.35
 654 | Amy    | 3.9 |    1000 |       3.90
 543 | Craig  | 3.4 |    2000 |       6.80
(12 rows)
```
