# Introduction and Relational Databases
> :dvd: Annotations from the course [Introduction and Relational Databases](https://lagunita.stanford.edu/courses/DB/RDB/SelfPaced/info) - Stanford

## Table of Contents
 - [Introduction](#introduction)
 - [The Relational Model](#the-relational-model)
 - [Querying Relational Databases](#querying-relational-databases)

## Introduction
### Key Concepts
- Data model
  - How the data is structured
  - Set of records
- Schema versus data
  - The schema sets up the structure of the database
  - Data is the actual data stored within the schema
- Data definition language (DDL)
  - Schema is set in the beginning and doesn't change very much where the data changes rapidly
- Data manipulation or query language (DML)
  - Querying and modifying the database

### Key people
- DBMS implementer
  - Builds the database system
- Database designer
  - Person who establishes the schema for a database
  - Responsible for figure out how to structure the data before starting the development of the application
  - It's a surprisingly difficult job when you have very complex data involved
- Database application developer
  - Build programs that are going to run on the database, often interfacing between the eventual user and the data itself
  - Different programs can run on the same database.
    - E.g: sales database where some applications are inserting sales as they happen while others are analyzing the sales.
- Database administrator
  - Person who loads the data, gets the whole thing running and keeps it running smoothly
  - Very important job for large database applications
  - DB systems tend to have a number of tunning parameters associated with them
  - Tunning parameters right can make a significant difference in the performance of the DB system

## The Relational Model
- Database = set of named **relations** (or **tables**)
- Each relation has a set of named **attributes** (or **columns**)
- Each **tuple** (or **row**) has a value for each attribute
- Each attribute has a **type** (or **domain**)

**Student**

ID | name | GPA | photo
:--: | :--: | :--: | :--:
123 | Amy | 3.9 | :smile:
234 | Bob | 3.4 | :disappointed:

**College**

name | state | enrollment
:--: | :--: | :--:
stanford | CA | 15,000
Berkeley | CA | 36,000
MIT | MA | 10,000

- **Schema**: structural description of relations in database
- **Instance**: actual contents at given point in time
- **NULL**: special value for "unknown" or "undefined"
- **Key**: attribute whose value is unique in each tuple, or set of attributes whose combined values are unique. E.g:
  - Student table -> key: `id`
  - College table -> key: `name state`

### Creating relations (tables) in SQL

```sql
Create Table Student(ID, name, GPA, photo)

Create Table College(name string, state char(2), enrollment integer)
```

- Very simple model
- Query with high-level languages: simple yet expressive
- Efficient implementations

## Querying Relational Databases
