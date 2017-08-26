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

> **OBS**: we can add the `WHERE` clauses in the `ON` "space". One hint when we don't do this is the same as implicitly say: "here is the condition that really applies to the combinations of the tuples, and the rest of the conditions apply to separate attributes".

**Query**: Project the student ID, student name, GPA, college name and enrollment combinying three tables (apply, student and college), and combine tuples only when student ID matches apply ID, and the apply cname matches the college cname.

```sql
SELECT student.sid, student.sname, gpa, college.cname, enrollment
FROM student, college, apply
WHERE student.sid = apply.sid
AND college.cname = apply.cname;
```

**Result**:

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

**Query**: equal to previous

```sql
SELECT student.sid, student.sname, gpa, college.cname, enrollment
FROM (student JOIN apply ON student.sid = apply.sid)
JOIN college
ON college.cname = apply.cname;
```

**Result**:

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

Reminder of relational algebra: the natural join takes two relations that have column names in common and performs a cross product that only keeps the tuples where the tuples have the same value, in those common attribute names.

**Query**:

```sql
SELECT DISTINCT sname, major
FROM student NATURAL JOIN apply;

# Equals to
# SELECT DISTINCT sname, major
# FROM student, apply
# WHERE student.sid = apply.sid;
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

**Query**:

```sql
SELECT * FROM student NATURAL JOIN apply;
```

**Result**:

```
 sid | sname | gpa | size_hs |  cname   |     major      | decision
-----+-------+-----+---------+----------+----------------+----------
 123 | Amy   | 3.9 |    1000 | Cornell  | EE             | y
 123 | Amy   | 3.9 |    1000 | Berkeley | CS             | y
 123 | Amy   | 3.9 |    1000 | Stanford | EE             | n
 123 | Amy   | 3.9 |    1000 | Stanford | CS             | y
 234 | Bob   | 3.6 |    1500 | Berkeley | biology        | n
 345 | Craig | 3.5 |     500 | Cornell  | EE             | n
 345 | Craig | 3.5 |     500 | Cornell  | CS             | y
 345 | Craig | 3.5 |     500 | Cornell  | bioengineering | n
 345 | Craig | 3.5 |     500 | MIT      | bioengineering | y
 678 | Fay   | 3.8 |     200 | Stanford | history        | y
 987 | Helen | 3.7 |     800 | Berkeley | CS             | y
 987 | Helen | 3.7 |     800 | Stanford | CS             | y
 876 | Irene | 3.9 |     400 | MIT      | marine biology | n
 876 | Irene | 3.9 |     400 | MIT      | biology        | y
 876 | Irene | 3.9 |     400 | Stanford | CS             | n
 765 | Jay   | 2.9 |    1500 | Stanford | history        | y
 765 | Jay   | 2.9 |    1500 | Cornell  | psychology     | y
 765 | Jay   | 2.9 |    1500 | Cornell  | history        | n
 543 | Craig | 3.4 |    2000 | MIT      | CS             | n
(19 rows)
```

There is a feature that goes along with the join operator in SQL that's actually considered best pratice than using the natural join and it's called the `USING` clause.

The using clause explicitly lists the attributes that should be equated across the two relations.

In real applications, there can also be often 40, 50, even a hundred attributes in a relationship, so there is a high chance that you really could have attributes with the same name but aren't meant to be equated.

**Query**:

```sql
SELECT sname, gpa
FROM student JOIN apply USING(sid)
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

**Query**: Combine two instances of the student relation in order to find pairs of students that have the same GPA. Project students' ID, name and GPA.

```sql
SELECT s1.sid, s1.sname, s1.gpa, s2.sid, s2.sname, s2.gpa
FROM student s1 JOIN student s2 USING(gpa)
WHERE s1.sid < s2.sid;
```

**Result**:

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

**Query**: Show name and id of students, and the college name and major where they applied to.

```sql
SELECT sname, sid, cname, major
FROM student JOIN apply USING(sid);
```

**Result**:

```
 sname | sid |  cname   |     major
-------+-----+----------+----------------
 Amy   | 123 | Cornell  | EE
 Amy   | 123 | Berkeley | CS
 Amy   | 123 | Stanford | EE
 Amy   | 123 | Stanford | CS
 Bob   | 234 | Berkeley | biology
 Craig | 345 | Cornell  | EE
 Craig | 345 | Cornell  | CS
 Craig | 345 | Cornell  | bioengineering
 Craig | 345 | MIT      | bioengineering
 Fay   | 678 | Stanford | history
 Helen | 987 | Berkeley | CS
 Helen | 987 | Stanford | CS
 Irene | 876 | MIT      | marine biology
 Irene | 876 | MIT      | biology
 Irene | 876 | Stanford | CS
 Jay   | 765 | Stanford | history
 Jay   | 765 | Cornell  | psychology
 Jay   | 765 | Cornell  | history
 Craig | 543 | MIT      | CS
(19 rows)
```

**Query**: Using the previous query, we also want in our result the information about students who haven't yet applied anywhere. To do this, we need to use the clause `LEFT OUTER JOIN`. We can omit the `OUTER` keyword.

```sql
SELECT sname, sid, cname, major
FROM student LEFT JOIN apply USING(sid);
```

**Result**:

```
 sname  | sid |  cname   |     major
--------+-----+----------+----------------
 Amy    | 123 | Cornell  | EE
 Amy    | 123 | Berkeley | CS
 Amy    | 123 | Stanford | EE
 Amy    | 123 | Stanford | CS
 Bob    | 234 | Berkeley | biology
 Craig  | 345 | Cornell  | EE
 Craig  | 345 | Cornell  | CS
 Craig  | 345 | Cornell  | bioengineering
 Craig  | 345 | MIT      | bioengineering
 Doris  | 456 |          |
 Edward | 567 |          |
 Fay    | 678 | Stanford | history
 Gary   | 789 |          |
 Helen  | 987 | Berkeley | CS
 Helen  | 987 | Stanford | CS
 Irene  | 876 | MIT      | marine biology
 Irene  | 876 | MIT      | biology
 Irene  | 876 | Stanford | CS
 Jay    | 765 | Stanford | history
 Jay    | 765 | Cornell  | psychology
 Jay    | 765 | Cornell  | history
 Amy    | 654 |          |
 Craig  | 543 | MIT      | CS
(23 rows)
```

**Query**: Retain apply tuples whether or not they matched a student tuple.

```sql
SELECT sname, sid, cname, major
FROM student RIGHT JOIN apply USING(sid);
```

**Result**:

```
 sname | sid |  cname   |     major
-------+-----+----------+----------------
 Amy   | 123 | Cornell  | EE
 Amy   | 123 | Berkeley | CS
 Amy   | 123 | Stanford | EE
 Amy   | 123 | Stanford | CS
 Bob   | 234 | Berkeley | biology
 Craig | 345 | Cornell  | EE
 Craig | 345 | Cornell  | CS
 Craig | 345 | Cornell  | bioengineering
 Craig | 345 | MIT      | bioengineering
 Fay   | 678 | Stanford | history
 Helen | 987 | Berkeley | CS
 Helen | 987 | Stanford | CS
 Irene | 876 | MIT      | marine biology
 Irene | 876 | MIT      | biology
 Irene | 876 | Stanford | CS
 Jay   | 765 | Stanford | history
 Jay   | 765 | Cornell  | psychology
 Jay   | 765 | Cornell  | history
 Craig | 543 | MIT      | CS
       | 321 | MIT      | psychology
       | 321 | MIT      | history
(21 rows)
```

**Query**: Have unmatched tuples from both left and right appearing in the result.

```sql
SELECT sname, sid, cname, major
FROM student FULL JOIN apply USING(sid);
```

**Result**:

```
 sname  | sid |  cname   |     major
--------+-----+----------+----------------
 Amy    | 123 | Cornell  | EE
 Amy    | 123 | Berkeley | CS
 Amy    | 123 | Stanford | EE
 Amy    | 123 | Stanford | CS
 Bob    | 234 | Berkeley | biology
 Craig  | 345 | Cornell  | EE
 Craig  | 345 | Cornell  | CS
 Craig  | 345 | Cornell  | bioengineering
 Craig  | 345 | MIT      | bioengineering
 Doris  | 456 |          |
 Edward | 567 |          |
 Fay    | 678 | Stanford | history
 Gary   | 789 |          |
 Helen  | 987 | Berkeley | CS
 Helen  | 987 | Stanford | CS
 Irene  | 876 | MIT      | marine biology
 Irene  | 876 | MIT      | biology
 Irene  | 876 | Stanford | CS
 Jay    | 765 | Stanford | history
 Jay    | 765 | Cornell  | psychology
 Jay    | 765 | Cornell  | history
 Amy    | 654 |          |
 Craig  | 543 | MIT      | CS
        | 321 | MIT      | psychology
        | 321 | MIT      | history
(25 rows)
```
