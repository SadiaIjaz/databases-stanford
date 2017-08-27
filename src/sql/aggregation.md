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

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```

**Query**:

```sql

```

**Result**:

```

```
