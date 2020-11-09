---
title: Data Warehousing
date: 2019-01-27
tags: [big data]
categories: course notes
---



# The road to analytics

- OLTP: 
	- consistent and reliable record-keeping. 
	- look at detailed individual records (small part of database)
	- lots of writes (thousands of people write together, like new customers registering)
	- fully interactive (<1s)
	- value **Consistency**.
- OLAP: web analytics, sales analytics, management support.
	- look at historical summarized consolidated data ( analysis over large part of database)
	- lots of reads (even not writing)
	- slow interactive, like MapReduce.
	- Redundancy, we denormalized everything. 

# OLAP
A dataware house is a **subject-oriented, integrated, time-variant, nonvolatile** collection of data in support of management's **decision-making** process.  
- subject-oriented: customers, sales, products  
- integrated: databases are in hundreds/thousands/or even more. Hard to join these isolated databases. When to analyze ,copy these databases from all over isolated machines in a single machine.
- time-variant
- non-volatile: copy without any update. 

## Architecture
From different sources -> ETL-> get the derived data -> analyze/report/mine.  

## Data Model
### Cubes
- **Dimensions**: what the data looks like  
- aggregation: the first thing we can do with data  
- slicing:  take alongside the dimension
- Dicer:  take all of the dimensions: what you want to look into details. Organize the rows and columns.  

## ETHLing
- extract: triggers, gateways, log extraction
- transform: derivation, value transformation, cleaning, filter, split, merge, join  
- load: integrity constraints, sorting, build indices, partition

## Implementation
- ROLAP: physically stored in RDMS. Fact table: one value in each row, with multi measure.  **Star schema**. snow-flake schema.
- MOLAP: the physical layer would be the memory-order harddrive.

### Query
MDX(multi-dimensional expressions): the language for cubes. Actually, a lot of people use SQL to query cubes.  

**GROUPING SETS, ROLLUP, CUBE**: ROLLUP: from left to right.

A cube is a list of dimensions indexing a list of measures.  
- hierarchies in dimensions: dimension values are organized in hierarchies. MDX does know this hierarchy.

### Statements
- dicing: ```SELECT DIM1 ON COLUMNS, DIM2 ON ROWS FROM ...```
- slicing: ```WHERE```
### Syntax: XBRL
based on XML  

The feeling: XML schema can be used to describe graphs, cubes.
