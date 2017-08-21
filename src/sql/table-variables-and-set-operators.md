# SQL- Table Variables and Set Operators
> :dvd: Annotations from the course [SQL](https://lagunita.stanford.edu/courses/DB/SQL/SelfPaced/info) - Stanford

- Table variables are in the `FROM` clause and serve two uses:
  1. Make queries more redable
  1. Rename relations that are used in the FROM clause, particularly when we have two instances of the same relation.

- Set operators:
  - Union operator
  - Intersect operator
  - Except operator (minus operator)

Query: Add a variable for each of the relation names.

```sql
SELECT s.sid, sname, gpa, a.cname, enrollment
FROM student s, college c, apply a
WHERE a.sid = s.sid
AND a.cname = c.cname;
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

Query: Look at where table variables are actually useful. When you need to have two instances of the same relation, you can have two different variables for the same relation.

```sql
SELECT s1.sid, s1.sname, s1.gpa, s2.sid, s2.sname, s2.gpa
FROM student s1, student s2
WHERE s1.gpa = s2.gpa;
```

Result:

```
 sid | sname  | gpa | sid | sname  | gpa
-----+--------+-----+-----+--------+-----
 123 | Amy    | 3.9 | 654 | Amy    | 3.9
 123 | Amy    | 3.9 | 876 | Irene  | 3.9
 123 | Amy    | 3.9 | 456 | Doris  | 3.9
 123 | Amy    | 3.9 | 123 | Amy    | 3.9
 234 | Bob    | 3.6 | 234 | Bob    | 3.6
 345 | Craig  | 3.5 | 345 | Craig  | 3.5
 456 | Doris  | 3.9 | 654 | Amy    | 3.9
 456 | Doris  | 3.9 | 876 | Irene  | 3.9
 456 | Doris  | 3.9 | 456 | Doris  | 3.9
 456 | Doris  | 3.9 | 123 | Amy    | 3.9
 567 | Edward | 2.9 | 765 | Jay    | 2.9
 567 | Edward | 2.9 | 567 | Edward | 2.9
 678 | Fay    | 3.8 | 678 | Fay    | 3.8
 789 | Gary   | 3.4 | 543 | Craig  | 3.4
 789 | Gary   | 3.4 | 789 | Gary   | 3.4
 987 | Helen  | 3.7 | 987 | Helen  | 3.7
 876 | Irene  | 3.9 | 654 | Amy    | 3.9
 876 | Irene  | 3.9 | 876 | Irene  | 3.9
 876 | Irene  | 3.9 | 456 | Doris  | 3.9
 876 | Irene  | 3.9 | 123 | Amy    | 3.9
 765 | Jay    | 2.9 | 765 | Jay    | 2.9
 765 | Jay    | 2.9 | 567 | Edward | 2.9
 654 | Amy    | 3.9 | 654 | Amy    | 3.9
 654 | Amy    | 3.9 | 876 | Irene  | 3.9
 654 | Amy    | 3.9 | 456 | Doris  | 3.9
 654 | Amy    | 3.9 | 123 | Amy    | 3.9
 543 | Craig  | 3.4 | 543 | Craig  | 3.4
 543 | Craig  | 3.4 | 789 | Gary   | 3.4
(28 rows)
```

Query: Previous query but with students that have different IDs.

```sql
SELECT s1.sid, s1.sname, s1.gpa, s2.sid, s2.sname, s2.gpa
FROM student s1, student s2
WHERE s1.gpa = s2.gpa
AND s1.sid <> s2.sid;
```

Result:

```
 sid | sname  | gpa | sid | sname  | gpa
-----+--------+-----+-----+--------+-----
 123 | Amy    | 3.9 | 654 | Amy    | 3.9
 123 | Amy    | 3.9 | 876 | Irene  | 3.9
 123 | Amy    | 3.9 | 456 | Doris  | 3.9
 456 | Doris  | 3.9 | 654 | Amy    | 3.9
 456 | Doris  | 3.9 | 876 | Irene  | 3.9
 456 | Doris  | 3.9 | 123 | Amy    | 3.9
 567 | Edward | 2.9 | 765 | Jay    | 2.9
 789 | Gary   | 3.4 | 543 | Craig  | 3.4
 876 | Irene  | 3.9 | 654 | Amy    | 3.9
 876 | Irene  | 3.9 | 456 | Doris  | 3.9
 876 | Irene  | 3.9 | 123 | Amy    | 3.9
 765 | Jay    | 2.9 | 567 | Edward | 2.9
 654 | Amy    | 3.9 | 876 | Irene  | 3.9
 654 | Amy    | 3.9 | 456 | Doris  | 3.9
 654 | Amy    | 3.9 | 123 | Amy    | 3.9
 543 | Craig  | 3.4 | 789 | Gary   | 3.4
(16 rows)
```

Query: Previous query but with students that have different IDs AND without repeat them in the inverse order (using `<` to do the trick).

```sql
SELECT s1.sid, s1.sname, s1.gpa, s2.sid, s2.sname, s2.gpa
FROM student s1, student s2
WHERE s1.gpa = s2.gpa
AND s1.sid < s2.sid;
```

Result:

```
 sid | sname  | gpa | sid | sname | gpa
-----+--------+-----+-----+-------+-----
 123 | Amy    | 3.9 | 654 | Amy   | 3.9
 123 | Amy    | 3.9 | 876 | Irene | 3.9
 123 | Amy    | 3.9 | 456 | Doris | 3.9
 456 | Doris  | 3.9 | 654 | Amy   | 3.9
 456 | Doris  | 3.9 | 876 | Irene | 3.9
 567 | Edward | 2.9 | 765 | Jay   | 2.9
 654 | Amy    | 3.9 | 876 | Irene | 3.9
 543 | Craig  | 3.4 | 789 | Gary  | 3.4
(8 rows)
```

Next query: Start using Set operators. Let's use the union operator to generate a list that includes names of colleges together with names of students.

```sql
SELECT cname FROM college
UNION
SELECT sname from student;
```

Result

```
  cname
----------
 Helen
 Doris
 Gary
 Fay
 Edward
 Berkeley
 MIT
 Craig
 Irene
 Jay
 Bob
 Cornell
 Amy
 Stanford
(14 rows)
```

Next query: rename the column of the previous query.

```sql
SELECT cname as name FROM college
UNION
SELECT sname as name FROM student;
```

Result:

```
   name
----------
 Helen
 Doris
 Gary
 Fay
 Edward
 Berkeley
 MIT
 Craig
 Irene
 Jay
 Bob
 Cornell
 Amy
 Stanford
(14 rows)
```

Next query: By default, SQL eliminates duplicates in its results when using the union operator. To change it we need to use the `ALL` keyword.

```sql
SELECT cname as name FROM college
UNION ALL
SELECT sname as name FROM student;
```

Result

```
   name
----------
 Stanford
 Berkeley
 MIT
 Cornell
 Amy
 Bob
 Craig
 Doris
 Edward
 Fay
 Gary
 Helen
 Irene
 Jay
 Amy
 Craig
(16 rows)
```

Next query: to sort the last query, add the `ORDER BY <column>` clause.

```sql
SELECT cname as name FROM college
UNION ALL
SELECT sname as name FROM student
ORDER BY name;
```

```
   name
----------
 Amy
 Amy
 Berkeley
 Bob
 Cornell
 Craig
 Craig
 Doris
 Edward
 Fay
 Gary
 Helen
 Irene
 Jay
 MIT
 Stanford
(16 rows)
```

Next query: demonstrates the intersect operator. Let's get the IDs of all students who have applied to both CS for a major and EE for a major.

```sql
SELECT sid FROM apply WHERE major = 'CS'
INTERSECT
SELECT sid FROM apply WHERE major = 'EE';
```

Result

```
 sid
-----
 123
 345
(2 rows)
```

Next query: some databases sytems don't support the intersect operator. In that case we just have to write our query in different ways.

```sql
SELECT a1.sid
FROM apply a1, apply a2
WHERE a1.sid = a2.sid
  AND a1.major = 'CS'
  AND a2.major = 'EE';
```

Result

```
 sid
-----
 123
 123
 123
 123
 345
(5 rows)
```

Next query: to elimate duplicates from the previous query, add the `DISTINCT` keyword.

```sql
SELECT DISTINCT a1.sid
FROM apply a1, apply a2
WHERE a1.sid = a2.sid
  AND a1.major = 'CS'
  AND a2.major = 'EE';
```

Result

```
 sid
-----
 123
 345
(2 rows)
```

Next query: use the EXCEPT (minus - difference) operator. Let's find the students who applied to CS but did not apply to EE.

```sql
SELECT sid from apply WHERE major = 'CS'
EXCEPT
SELECT sid from apply WHERE major = 'EE';
```

Result

```
 sid
-----
 543
 876
 987
(3 rows)
```
