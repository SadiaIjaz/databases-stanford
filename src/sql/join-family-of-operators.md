# SQL - The Join Family of Operators
> :dvd: Annotations from the course [SQL](https://lagunita.stanford.edu/courses/DB/SQL/SelfPaced/info) - Stanford

## Join types
- **Inner Join on Condition**
  - Take the cross product and apply a condition, only keeping the tuples that satisfy the condition.
- **Natural Join**
  - Equates columns across tables of the same name.
  - Requires the values in those columns to be the same to keep tuple in the cross product
- **Inner Join Using(attrs)**
  - It's like the Natural Join except you explicitly list the attributes you want to be equated.
- **Left | Right | Full Outer Join**
  - The most interesting type.
  - Combine tuples like in the *Inner Join on Condition* (theta join) except when tuples don't match the theta condition, they are still added to the result and padded with no values.

None of these operators are actually adding expressive power to SQL. All of them can be expressed using other constructs, but they can be quite useful in formulating queries, especially the outer join, because some queries are fairly complicated to express without it.

**Query**: Match student names with majors to which they have applied.

```sql
SELECT DISTINCT sname, major
FROM student, apply
WHERE student.sid = apply.sid;
```

**Result**:

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
 Amy   | EE
 Jay   | psychology
 Helen | CS
 Jay   | history
 Bob   | biology
 Amy   | CS
(13 rows)
```

Query: Match student names with majors to which they have applied.

```sql
SELECT DISTINCT sname, major
FROM student INNER JOIN apply
ON student.sid = apply.sid;
```

**Result**:

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
 Amy   | EE
 Jay   | psychology
 Helen | CS
 Jay   | history
 Bob   | biology
 Amy   | CS
(13 rows)
```

> **OBS**: In SQL, `INNER JOIN` and `JOIN` are equivalents so you can omit the word `INNER`.

**Query**: Find the name and GPA of students who came from a high school with less than 1000 students and have applied to a major in computer science at Stanford.

```sql
SELECT sname, gpa
FROM student, apply
WHERE size_hs < 1000
AND major = 'CS'
AND cname = 'Stanford'
AND student.sid = apply.sid;
```

**Result**:

```
 sname | gpa
-------+-----
 Helen | 3.7
 Irene | 3.9
(2 rows)
```

**Query**: Find the name and GPA of students who came from a high school with less than 1000 students and have applied to a major in computer science at Stanford.

```sql
SELECT sname, gpa
FROM student JOIN apply
ON student.sid = apply.sid
WHERE size_hs < 1000
AND major = 'CS'
AND cname = 'Stanford';
```

**Result**:

```
 sname | gpa
-------+-----
 Helen | 3.7
 Irene | 3.9
(2 rows)
```

> **OBS**: we can add the `WHERE` clauses in the `ON` "space". One hint when we don't do this is by implicit say: "here is the condition that really applies to the combinations of the tuples, and the rest of the conditions apply to separete attributes".

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
