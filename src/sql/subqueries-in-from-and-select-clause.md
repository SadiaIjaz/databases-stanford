# SQL- Subqueries in FROM and SELECT clause
> :dvd: Annotations from the course [SQL](https://lagunita.stanford.edu/courses/DB/SQL/SelfPaced/info) - Stanford

Query: Return all students where scaling their GPA changes its value by more than one, specifically either the scale GPA minus the GPA is greater than one or the GPA minus the scale GPA is greater than one.

```sql
SELECT sid, sname, gpa, gpa * (size_hs / 1000.0) AS scaled_gpa
FROM student
WHERE gpa * (size_hs / 1000.0) - gpa > 1.0
OR gpa - gpa * (size_hs / 1000.0) > 1.0;
```

Result

```
 sid | sname  | gpa |    scaled_gpa
-----+--------+-----+-------------------
 234 | Bob    | 3.6 |  5.39999985694885
 345 | Craig  | 3.5 |              1.75
 567 | Edward | 2.9 |  5.80000019073486
 678 | Fay    | 3.8 | 0.759999990463257
 876 | Irene  | 3.9 |  1.56000003814697
 765 | Jay    | 2.9 |  4.35000014305115
 543 | Craig  | 3.4 |  6.80000019073486
(7 rows)
```

Next query: we can simplify the previous clause by using the `ABS()` function that's built into most SQL implementations.

```sql
SELECT sid, sname, gpa, gpa * (size_hs / 1000.0) AS scaled_gpa
FROM student
WHERE ABS(gpa * (size_hs / 1000.0) - gpa) > 1.0;
```

Result

```
 sid | sname  | gpa |    scaled_gpa
-----+--------+-----+-------------------
 234 | Bob    | 3.6 |  5.39999985694885
 345 | Craig  | 3.5 |              1.75
 567 | Edward | 2.9 |  5.80000019073486
 678 | Fay    | 3.8 | 0.759999990463257
 876 | Irene  | 3.9 |  1.56000003814697
 765 | Jay    | 2.9 |  4.35000014305115
 543 | Craig  | 3.4 |  6.80000019073486
(7 rows)
```

Next query: refactor last query to use subquery in the FROM clause.

```sql
SELECT *
FROM (
  SELECT sid, sname, gpa, gpa * (size_hs / 1000.0) as scaled_gpa
  FROM Student
) G
WHERE abs(G.scaled_gpa - gpa) > 1.0;
```

Result:

```
 sid | sname  | gpa |    scaled_gpa
-----+--------+-----+-------------------
 234 | Bob    | 3.6 |  5.39999985694885
 345 | Craig  | 3.5 |              1.75
 567 | Edward | 2.9 |  5.80000019073486
 678 | Fay    | 3.8 | 0.759999990463257
 876 | Irene  | 3.9 |  1.56000003814697
 765 | Jay    | 2.9 |  4.35000014305115
 543 | Craig  | 3.4 |  6.80000019073486
(7 rows)
```

Next query: Find colleges and pair those colleges with the highest GPA among their applicants.

```sql
SELECT DISTINCT college.cname, state, gpa
FROM college, apply, student
WHERE college.cname = apply.cname
AND apply.sid = student.sid
AND gpa >= all (
  SELECT gpa FROM student, apply
  WHERE student.sid = apply.sid
  AND apply.cname = college.cname
);
```

Result

```
  cname   | state | gpa
----------+-------+-----
 Stanford | CA    | 3.9
 Berkeley | CA    | 3.9
 MIT      | MA    | 3.9
 Cornell  | NY    | 3.9
(4 rows)
```

Next query: previous query using subqueries in the select clause.

```sql
SELECT cname, state, (
  SELECT DISTINCT gpa
  FROM apply, student
  WHERE college.cname = apply.cname
  AND apply.sid = student.sid
) as gpa
FROM college;
```