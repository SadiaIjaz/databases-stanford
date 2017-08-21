# SQL- Subqueries in WHERE clause
> :dvd: Annotations from the course [SQL](https://lagunita.stanford.edu/courses/DB/SQL/SelfPaced/info) - Stanford

- Sub-queries are nested `SELECT` statements within the condition (`WHERE`), and they are quite poweful.

First query: Finds the IDs and names of all students who have applied to major in CS at some college.

```sql
SELECT sid, sname
FROM student
WHERE sid IN (
  SELECT sid
  FROM apply
  WHERE major = 'CS'
);
```

Result:

```
 sid | sname
-----+-------
 123 | Amy
 345 | Craig
 543 | Craig
 876 | Irene
 987 | Helen
(5 rows)
```

Next query: the same previous query without a subquery.

```sql
SELECT DISTINCT student.sid, sname
FROM student, apply
WHERE student.sid = apply.sid
AND major = 'CS';
```

Result

```
 sid | sname
-----+-------
 123 | Amy
 543 | Craig
 345 | Craig
 987 | Helen
 876 | Irene
(5 rows)
```

Next query: Show the GPA of students that applied for CS. In that case, we can't use the join version of the query, we need to use a subquery to list all GPA without get duplicates for the same student or omit some results using `DISTINCT`.

```sql
SELECT GPA
FROM student
WHERE sid IN (
  SELECT sid
  FROM apply
  WHERE major = 'CS'
);
```

Result

```
 gpa
-----
 3.9
 3.5
 3.4
 3.9
 3.7
(5 rows)
```

Next query: simulate the `EXCEPT` set operator using subqueries. (Equal to the last query of the previous lecture). Find all students that applied to CS and not applied to EE.

```sql
SELECT sid, sname
FROM student
WHERE sid IN (SELECT sid FROM apply WHERE major = 'CS')
AND NOT sid IN (SELECT sid FROM apply WHERE major = 'EE');
```

Result:

```
 sid | sname
-----+-------
 543 | Craig
 876 | Irene
 987 | Helen
(3 rows)
```

Next query: Find all colleges such there's some other college that is in the same state.

```sql
SELECT cname, state
FROM college c1
WHERE EXISTS (
  SELECT * FROM college c2
  WHERE c2.state = c1.state
  AND c1.cname <> c2.cname
);
```

Result

```
  cname   | state
----------+-------
 Stanford | CA
 Berkeley | CA
(2 rows)
```

Next query: find the college that has the largest enrollment.

```sql
select cname
from college c1
where not exists (
  select * from college c2
  where c2.enrollment > c1.enrollment
);
```

Result

```
  cname
----------
 Berkeley
(1 row)
```

Next query: find the student with the highest GPA.

```sql
select sname, gpa
from student s1
where not exists (
  select * from student s2
  where s2.gpa > s1.gpa
);
```

Result

```
 sname | gpa
-------+-----
 Amy   | 3.9
 Doris | 3.9
 Irene | 3.9
 Amy   | 3.9
(4 rows)
```

Next query: This query uses the `ALL` keyword. Let's create a query like the previous one using `ALL`.

```sql
select sname, gpa
from student
where gpa >= all (select gpa from student);
```

Result

```
 sname | gpa
-------+-----
 Amy   | 3.9
 Doris | 3.9
 Irene | 3.9
 Amy   | 3.9
(4 rows)
```

Next query: find the college with the highest enrollment price.

```sql
select cname
from college c1
where enrollment > all (
  select enrollment from college c2
  where c2.cname <> c1.cname
);
```

Result

```
  cname
----------
 Berkeley
(1 row)
```

Next query: find the college with the highest enrollment price using `ANY`.

```sql
select cname
from college c1
where not enrollment <= any (
  select enrollment from college c2
  where c2.cname <> c1.cname
);
```

Result

```
  cname
----------
 Berkeley
(1 row)
```

Next query: Find all students where the size of their high school is greater than `any` high school size.

```sql
select sid, sname, size_hs
from student
where size_hs > any (
  select size_hs from student
);
```

Result

```
 sid | sname  | size_hs
-----+--------+---------
 123 | Amy    |    1000
 234 | Bob    |    1500
 345 | Craig  |     500
 456 | Doris  |    1000
 567 | Edward |    2000
 789 | Gary   |     800
 987 | Helen  |     800
 876 | Irene  |     400
 765 | Jay    |    1500
 654 | Amy    |    1000
 543 | Craig  |    2000
(11 rows)
```

Next query: we can use `EXISTS` and `NOT EXISTS` instead of `ANY | ALL`.

```sql
select sid, sname, size_hs
from student s1
where exists (
  select * from student s2
  where s2.size_hs < s1.size_hs
);
```

Result

```
test_db(> );
 sid | sname  | size_hs
-----+--------+---------
 123 | Amy    |    1000
 234 | Bob    |    1500
 345 | Craig  |     500
 456 | Doris  |    1000
 567 | Edward |    2000
 789 | Gary   |     800
 987 | Helen  |     800
 876 | Irene  |     400
 765 | Jay    |    1500
 654 | Amy    |    1000
 543 | Craig  |    2000
(11 rows)
```
