# Relational Algebra
> :dvd: Annotations from the course [Relational Algebra](https://lagunita.stanford.edu/courses/DB/RA/SelfPaced/info) - Stanford

## Table of Contents
- [Select, Project, Join](#select---project---join)
- [Set Operators, Renaming, Notation](#set-operators---renaming---notation)

> [Mathematical operators and symbols in Unicode](https://en.wikipedia.org/wiki/Mathematical_operators_and_symbols_in_Unicode)

## Select - Project - Join
- Queries over relational databases operate on relations (tables) and they also produce relations as a result.

Examples: simple college admissions database

Table 1: College(**cName**, state, enrollment) <br>
Table 2: Student(**sID**, sName, GPA, sizeHS) <br>
Table 3: Apply(**sID**, **cName**, **major**, decision) <br>

- Simples query: relation name
  - Ex: `Student` -> We'll get a copy of the Student relation
- Use **operators** to filter, slice and combine relations

### Operators
- **Select**
  - Picks a certain rows
  - Is denotaded by a sigma (`σ`) with a subscript, that's a condition used to filter the rows that we extract from the relations.
  - Examples:
    - Students with GPA > 3.7
    - σ<sub>GPA > 3.7</sub> Student
    - Students with GPA > 3.7 and sizeHS < >
    - σ<sub>GPA > 3.7 ⋀ sizeHS < 1000</sub> Student
    - Applications to Stanford CS major
    - σ<sub>GPA > 3.7 ⋀ sizeHS < 1000</sub> Student

- **Project**
  - Picks certain columns
  - Examples:
    - ID and decision of all applications
    - π<sub>sID, decision</sub> Apply

- To pick both rows and columns
  - ID and name of students with GPA > 3.7
  - π<sub>sID, sName</sub>(σ<sub>GPA > 3.7</sub> Student)

- **Cross-product**
  - Combine two relations (aka Cartesian product)
  - Example:
    - Names and GPAs of students with sizeHS > 1000 who applied to CS and were rejected
    - π<sub>sName, GPA</sub>(σ<sub>student.sID = apply.sID ⋀ sizeHS > 1000 ⋀ major = 'cs' ⋀ decision = 'rejected'</sub> (Student x Apply))

- **Natural Join** (`⋈` - bow tie)
  - Enforce equality on all attributes with the same name
  - Eliminate one copy of duplicate attributes/columns
  - Examples:
    - Names and GPAs of students with sizeHS > 1000 who applied to CS and were rejected
    - π<sub>sName, GPA</sub>(σ<sub>sizeHS > 1000 ⋀ major = 'cs' ⋀ decision = 'rejected'</sub> (Student ⋈ Apply))
    - Names and GPAs of students with sizeHS > 1000 who applied to CS at college with enrollment > 20,000 and were rejected
    - π<sub>sName, GPA</sub>(σ<sub>sizeHS > 1000 ⋀ major = 'cs' ⋀ decision = 'rejected' ⋀ enrollment > 20,000</sub> (Student ⋈ (Apply ⋈ College)))

- **Theta Join**
  - Exp<sub>1</sub> ⋈<sub>𝚹</sub> Exp<sub>2</sub> ≡ σ<sub>𝚹</sub> (Exp<sub>1</sub> x <sub></sub> Exp<sub>2</sub>)
  - Basic operation implemented in DBMS
  - Term "join" often means theta join

## Set Operators - Renaming - Notation

- Relational algebra query (expression) on set of relations produces relation as a result

Examples: simple college admissions database

Table 1: College(**cName**, state, enrollment) <br>
Table 2: Student(**sID**, sName, GPA, sizeHS) <br>
Table 3: Apply(**sID**, **cName**, **major**, decision) <br>

- **Union** operator
  - Example: list of college and student names
  - π<sub>cName</sub> College ⋃ π<sub>sName</sub> Student
