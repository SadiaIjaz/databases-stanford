# SQL - Aggregation
> :dvd: Annotations from the course [SQL](https://lagunita.stanford.edu/courses/DB/SQL/SelfPaced/info) - Stanford

"Aggregation" functions will appear in the `select` clause initially and what they
do is they perform computations over sets of values in multiple rows of our
relations, and the basic aggregation functions supported by every SQL system are
*minimum*, *maximum*, *some*, *average* and *count*.

We can also add two new clauses to the SQL `select` `from` `where` statement, the
`group by` and `having` clause. The `group by` allows us to partition our relations
into groups and then will compute aggregate functions over each group independently.
The `having` condition allows us to test filters on the results of aggregate values.
The `where` condition applies to single rows at a time. The `having` condition
will apply to the groups that we generate from the `group by` clause.

**Query**: calculate de average gpa of students.

```sql
SELECT AVG(gpa) as "AVG GPA" FROM student;
```

**Result**:

```
     AVG GPA
------------------
 3.56666672229767
(1 row)
```

**Query**: Find the minimum GPA of students who have applied to CS major.

```sql
SELECT MIN(gpa) as "Min GPA"
FROM student JOIN apply USING(sid)
WHERE major = 'CS';
```

**Result**:

```
 Min GPA
---------
     3.4
(1 row)
```

**Query**: To remove duplicate entries in the previous queries we need to use a
subquery.

```sql
SELECT * FROM student
WHERE sid in (
  SELECT sid FROM apply WHERE major = 'CS'
);
```

**Result**:

```
 sid | sname | gpa | size_hs
-----+-------+-----+---------
 123 | Amy   | 3.9 |    1000
 345 | Craig | 3.5 |     500
 543 | Craig | 3.4 |    2000
 876 | Irene | 3.9 |     400
 987 | Helen | 3.7 |     800
(5 rows)
```

**Query**: Now we can find the correct average gpa.

```sql
SELECT AVG(gpa) FROM student
WHERE sid IN (
  SELECT sid FROM apply WHERE major = 'CS'
);
```

**Result**:

```
       avg
-----------------
 3.6800000667572
(1 row)
```

**Query**: Find the number of colleges whose enrollment is greater than fifteen thousand.

```sql
SELECT COUNT FROM college WHERE enrollment > 15000;
```

**Result**:

```
 count
-------
     2
(1 row)
```

**Query**: find the number of students that applied to Cornell.

```sql
SELECT COUNT(DISTINCT sid) FROM apply WHERE cname = 'Cornell';
```

**Result**:

```
 count
-------
     3
(1 row)
```

**Query**: calculates if the average gpa of students that apply to CS is greater than the gpa of students that don't apply to CS.

```sql
SELECT cs.avgGPA - noncs.avgGPA as "CS vs NonCS"
FROM (
  SELECT AVG(GPA) AS avgGPA FROM student
  WHERE sid IN (
    SELECT sid FROM apply WHERE major = 'CS'
  )
) as cs,
(
  SELECT AVG(GPA) AS avgGPA FROM student
  WHERE sid NOT IN (
    SELECT sid FROM apply WHERE major = 'CS'
  )
) as noncs;
```

**Result**:

```
    CS vs NonCS
-------------------
 0.194285733359201
(1 row)
```

The `GROUP BY` clause is used only with aggregation.

**Query**: Find the number of applicants to each college using grouping.

```sql
SELECT cname, count(*)
FROM apply
GROUP BY cname;
```

**Result**:

```
  cname   | count
----------+-------
 MIT      |     6
 Cornell  |     6
 Berkeley |     3
 Stanford |     6
(4 rows)
```

**Query**: find the total enrollment of college students for each state.

```sql
SELECT state, SUM(enrollment)
FROM college
GROUP BY state;
```

**Result**:

```
 state |  sum
-------+-------
 CA    | 51000
 NY    | 21000
 MA    | 10000
(3 rows)
```

**Query**: Computes for each college and major combination, the minimum and maximum GPA for the students who have applied to that college.

```sql
SELECT cname, major, MIN(gpa), MAX(gpa)
FROM student JOIN apply USING(sid)
GROUP BY cname, major;
```

**Result**:

```
  cname   |     major      | min | max
----------+----------------+-----+-----
 Cornell  | psychology     | 2.9 | 2.9
 MIT      | biology        | 3.9 | 3.9
 Cornell  | history        | 2.9 | 2.9
 Stanford | EE             | 3.9 | 3.9
 Stanford | CS             | 3.7 | 3.9
 Berkeley | CS             | 3.7 | 3.9
 Berkeley | biology        | 3.6 | 3.6
 MIT      | marine biology | 3.9 | 3.9
 Stanford | history        | 2.9 | 3.8
 Cornell  | CS             | 3.5 | 3.5
 MIT      | CS             | 3.4 | 3.4
 Cornell  | bioengineering | 3.5 | 3.5
 MIT      | bioengineering | 3.5 | 3.5
 Cornell  | EE             | 3.5 | 3.9
(14 rows)
```

**Query**: Find the information about the spread of GPAs for each college. What the difference between the maximum and minimum.

```sql
SELECT mx - mn as "GPA Spread"
FROM (
  SELECT cname, major, MIN(gpa) as mn, MAX(gpa) as mx
  FROM student JOIN apply USING(sid)
  GROUP BY cname, major
) M;
```

**Result**:

```
 GPA Spread
------------
          0
          0
          0
          0
        0.2
        0.2
          0
          0
        0.9
          0
          0
          0
          0
        0.4
(14 rows)
```

**Query**: find the largest spread based on the last query.

```sql
SELECT MAX(mx - mn) as "GPA Spread"
FROM (
  SELECT cname, major, MIN(gpa) as mn, MAX(gpa) as mx
  FROM student JOIN apply USING(sid)
  GROUP BY cname, major
) M;
```

**Result**:

```
 GPA Spread
------------
        0.9
(1 row)
```

**Query**: Find the number of colleges that have been applied for the number of colleges that have been applied to by each student.

```sql
SELECT sid, sname, count(distinct cname)
FROM student JOIN apply USING(sid)
GROUP BY sid;
```

**Result**:

```
 sid | sname | count
-----+-------+-------
 123 | Amy   |     3
 234 | Bob   |     1
 345 | Craig |     2
 543 | Craig |     1
 678 | Fay   |     1
 765 | Jay   |     2
 876 | Irene |     2
 987 | Helen |     2
(8 rows)
```

**Query**: last query with students that didn't applied anywhere.

```sql
SELECT sid, sname, count(distinct cname)
FROM student JOIN apply USING(sid)
GROUP BY sid
UNION
SELECT sid, sname, 0
FROM student
WHERE sid NOT IN (select sid FROM apply);
```

**Result**:

```
 sid | sname  | count
-----+--------+-------
 876 | Irene  |     2
 456 | Doris  |     0
 789 | Gary   |     0
 678 | Fay    |     1
 123 | Amy    |     3
 234 | Bob    |     1
 543 | Craig  |     1
 987 | Helen  |     2
 654 | Amy    |     0
 567 | Edward |     0
 765 | Jay    |     2
 345 | Craig  |     2
(12 rows)
```

The `HAVING` clause only works with aggregation. Allows us apply conditions to the results of aggregate functions.

**Query**: Find all colleges that had less than 5 applicants

```sql
SELECT cname
FROM apply
GROUP BY cname
HAVING count(*) < 5;
```

**Result**:

```
  cname
----------
 Berkeley
(1 row)
```

**Query**: Find all majors represented in the database where the maximum GPA of a student applying for that major is lower than the average GPA in the database.

```sql
SELECT major
from student JOIN apply USING(sid)
GROUP BY major
HAVING MAX(gpa) < (
  SELECT AVG(gpa) FROM student
);
```

**Result**:

```
     major
----------------
 psychology
 bioengineering
(2 rows)
```
