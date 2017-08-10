# JSON Data
> :dvd: Annotations from the course [JSON Data](https://lagunita.stanford.edu/courses/DB/JSON/SelfPaced/about) - Stanford

## Table of Contents
- [Introduction to JSON Data](#introduction-to-json-data)
- [JSON Demo](#json-demo)

## Introduction to JSON Data
- Like XML, JSON can be taught as a data model
- An alternative to the relational data model
- More appropiate for semi-structured data
- JSON: **J**ava**S**cript **O**bject **N**otation
- Standard for "serializing" data objects, usually in files
- Human-readable, useful for data interchange
- Also useful for representing and storing semi-structured data
- Not tied to JavaScript
- Has parsers for many languages

### Basic Constructs
- Base values
  - number, string, boolean
- Objects `{}`
  - sets of key-value pairs
- Arrays `[]`
  - list of values

### Relational Model vs JSON

⚡ | Relational | JSON
:--: | :--: | :--:
**Structure** | Tables | Nested sets and arrays
**Schema** | Fixed in advance | "self-describing" / flexible
**Queries** | simple expressive languages | -
**Ordering** | None | Arrays
**Implementation** | Native systems | Coupled with programming languages; NoSQL systems

## JSON Demo
