# SQL
> :dvd: Annotations from the course [SQL](https://lagunita.stanford.edu/courses/DB/SQL/SelfPaced/info) - Stanford


## Table of Contents
- [Introduction to SQL](#introduction-to-sql)
- [Basic SELECT Statement](#basic-select-statement)
- []()
- []()
- []()
- []()
- []()
- []()
- []()

## Introduction to SQL
- Data Definition Language (DDL)
  - Create table, Drop table, ...
- Data Manipulation Language (DML)
  - Select, Insert, Delete, Update
- Other commands
  - indexes, constraints, views, triggers, transactions, authorization, ...

## Basic SELECT Statement

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
