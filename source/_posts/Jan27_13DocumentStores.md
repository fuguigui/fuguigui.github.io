---
title: Document Stores
date: 2019-01-27
tags: [courses, database, big data]
categories: Big Data
---



Document stores don't like joins. 

# How can we rebuild the stack with XML/JSON?
- forced the trees into a table. (Schema-based shredding)
- Store the tree by edge: each edge one row. It will be extremely slow to querying, because of plenty of joining.  
- Overall, turning a tree into a table is not a good idea. They have different shapes.

# NoSQL
- **validation** after the data was populated, which is not possible in RDMS because RDMS even not allows you to store invalid data.
- A **huge** collection (millions, billions, ..) of **small** trees. 

## ETL
Directly get data from some system, not ETL: **extract, transform, load**.  
- In SQL: the process: first create the table, design the schema, put the columns, put the domains, insert the values. Then, start querying it. Some products still first ETL, then query: MongoDB, Couchbase, Elasticsearch
### Comparison
Think the time of reading data is fixed, and the querying time is variable. ETL first takse reading time, then it needs less querying time. But No ETL keeps reading data while querying, so the variable time is much larger

#### ETL
- takse time to load the data first
- faster (querying time less)
- update possible
- proprietary formats

#### No ETL
- no time lost in loading the data
- slower (to query)
- read-only
- interoperable formats
- 
# Abbreviation
ETL: extract, transform, load  
BSON: Binary-JSON 

# Indices
- Primary: _id  
- secondary: other fields
## Hash indices
### limitation
- no support for range queries
- hash function not perfect in real life
- space requirements for collision avoidance

## B Tree
Different designs: values only on the leaves/on the all nodes, balanced or imbalanced. 
## B+ Tree
- we can have more children! Disks love block access.
- actual values only at the leaves.
- often have extra leaf pointers (from a leaf to the next leaf with adjacent father)
- #children between **d+1** and **2d+1**, d is the max number of children of the non-leaf nodes. In another way, d is the number of keys. 2 keys mean 3 children.

### Insertion
Each row of nodes is less than 2d. If it is already 2d and one more node is inserted, the row will be splited into two parts: one with d+1 nodes, the other with d nodes, and generates a new father-node. 

### Deletion
When a row is deleted only left with one node, it will draw its parent down and merge with the other adjacent child row of its parent.

## Query
- without indices: scan/filter in memory
- with indices: prefilter with index, then scan/filter in memory.

## Creation Index
- hash: ```db.**.createIndex({"key":"hash"})```
- B-tree: ```db.**.createIndex({"key":1})```
- compound:```db.**.createIndex({"key1":1,"key2":-1})``` sort order. **prefixes are implied**