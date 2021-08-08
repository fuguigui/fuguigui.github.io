---
title: Database Basics
date: 2019-01-22
tags: [System, Big Data]
categories: [Learning Notes]
---



# Table equivalent Concepts

table,collection  
attribute,column,field,property  
row,business object,item,entity,**document**,record  
primary key,row ID,name  

# Relational Algebra
From the set theory definition: **relation** is a subset of sets cartesian product.
## Queries
### Set queries
union, intersection,  substraction  
### Renaming queries
relation renaming,  attribute renaming
### Filter queries
selection (to row),projection(to column)
### Binary queries
cartesian product (each to each, e.g. n cartesian with m, get nm items) ,natural product,**theta product**(???)

## Others
grouping, sorting

# Normal Forms
to reduce redundancy  
## Consistency 
update, delete, insert anomaly
## 1st normal (NF1)
get the key, no inserted tables. only atomic values
## 2nd normal (NF2)
the whole key: non-key attributes do not depend on a strict subset of the key
## 3rd normal (NF3)
nothing but the key: non-key attributes do not depend on other non-key attributes.
## BCNF (Boyce-Codd Normal Form) (3.5NF)
attributes do not depend on other non-key attributes. A slightly stronger version of 3NF.

# SQL
SQL was initially developed at IBM by Donald D. Chamberlin and Raymond F. Boyce in the early 1970s.  
SQL: structured english query language.
## Features
- Declarative: set-based  
- Functional: the output is still relation.

## Queries in SQL
**duplicate elimination** union(```A union B``` ), intersection(```A intersect B```),  substraction (```A except B```)  
relation renaming,  attribute renaming (```as```)  
selection (```where```),projection(```select ... ```)  
cartesian product (each to each, e.g. n cartesian with m, get nm items) ,natural product,**theta product**(???), joining (```...left/right/full outer join ... on ...```), natural join: ```natural full outer join ... on ...```, is ```outer``` not ```out```
grouping (```group by```), sorting (```order by (asc/desc/null first)```)  
aggregation operators (```max/min/sum/avg/count```), post-aggregation selection(```having: having count(*)>2```)  

**where vs. having**: where: before grouping; having: after grouping.

# Terminology
Schema: DDL: Data Definition language (create table/scheme, or drop it)  
Data: DML: Data manipulation language (query, insert or remove rows)  
CRUD: create, read, update, delete

## Language landscape
Software Engineering: proto-imperative-language -> imperative language (JAVA)   
-> Databases: proto-declarative language(Apache Spark) -> functional/declarative language (SPARQL)  
-> AI: proto-here-is-an-example language (Tensorflow) -> here-is-an-example language (bonsai)

# Transaction
## ACID
old time
### Atomicity
Either the entire transaction is applied, or none of it.
### Consistency
After a transaction, a database is in a consistent state again.
### Isolation
A transaction feels like nobody else is **writing** to the database.
### Durability
Update made don't disappear again.

## CAP
new era
### Consistency
atomic: all the nodes see the same data.
### Availability
a database can be accessed at all times.

### Partition tolerance
the database continue to function even if the network gets partitioned.

which holds for DynamoDB? availability and partition tolerance, not consistency.
# Performance
optimize for writing intensive: **OLTP**: OnLineTransactionProcessing  
optimize for reading intensive: **OLAP**: OnLineAnalyticalProcessing

# Abbreviations
CRUD, ACID, CAP, OLTP, OLAP, SQL, DDL, DML