---
title: Introduction
date: 2019-01-22
tags: [big data]
categories: course notes
---

![pic](BD1.png)

Database prehistory:  

1. speaking/singing(expressing information).   
2. writing(recording).   
3. accounting (processing information)   
4. printing (broadcasting in large scale).  

Database history: 1960s: File system; 1970s: relational era; 1980s: object era; 2000s: NoSQLs  
# 3Vs:
volume, variety, velocity  
## Volume
**Prefix**: KMGTPEZY:   
kilo(3 zeros),mega (6 zeros),giga,tera,peta,exa,zetta,yotta.  
kibi(2^10),mebi(2^20),gibi,tebi,pebi,exbi,zebi,yobi.  
## Variety
Data shapes: table, tree, graph, cube, text
## Velocity
Velocity paramount factors: capacity, latency, **throughput**. Rule: logarithmic.   
From Capacity to throughput: **parallelize**.  
From throughput to latency: **batch processing**.

Teacher's definition: Big Data : technologies to store, manage and analyze data that is too large to fit on a single machine, while accommodating for the issue of growing discrepancy between capacity, throughput and latency.

# Course Overview
## Data in the large  
- Key-value stores (S3)  
- Distributed file systems (HDFS)  
- Distributed query processing (MapReduce, Spark)  
- Resource management (YARN)  
- Column stores (HBase)  
## Data in the small  
- Document stores (MongoDB)  
- Syntax (XML, JSON)  
- Data models, Schemas, Querying  
- Data in the very small  
- Data warehouses (OLAP, ROLAP, XBRL)  
- Graph databases (RDF)  

Learn from the past: data independence: logical data model and physical storage are independent.

Data Model: 1. what data looks like; 2. what you can do with that.  

Overall architecture: language/model/compute/storage.

## The stack
from bottom to the top
### Storage
local file system, NFS, GFS, HDFS, S3, Azure Blob Storage
### Encoding
ASCII, ISO-8859-1, UTF-8, BSON(**???**)
### Syntax
Text, CSV, XML, JSON, RDF/XML, Turtle, XBRL
### Data Models
Table: relational model  
Tress: XML Infoset, XDM  
Graphs: RDF  
Cubes: OLAP
### Validation
XML Schema, JSON Schema, Relational schemas, XBRL taxonomies
### Processing
two-phase: MapReduce  
DAG(**???**)-driven: Tez, Spark, Flink, Ray  
Elastic computing: EC2
### Indexing
key-value stores, hash indices, B-Trees, geographical indices(**???**), spatial indicies(**???**)
### Data Stores
RDBMS, MongoDB, CouchBase, Elastic Search, Hive, HBase, MarkLogic, Cassandra.
### Querying
SQL,XQuery,JSONiq, N1QL, MDX, SPARQL, REST APIs
### UI
Excel, Access, Tableau ...
