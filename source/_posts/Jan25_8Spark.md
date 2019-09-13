---
title: Spark
date: 2019-01-25
tags: [courses, database, big data]
categories: Big Data
---



# Spark

It is for full-DAG query processing. Its first-class citizen: RDD.

8 nodes, 16 cores per node and 128 GB of memory per node.

## RDD
HDFS, S3 ...**creation** ->RDD, RDD -**Transformation**->RDD, RDD -**action**->on screen
## Features
- lineage graph
- lazy evaluation: each action triggers an evaluation

## Transformation
- on single RDD: filter, map, flatMap, distinct, sample (fraction+seed)
- on two or more RDDs: union, intersection, subtract, cartesian product
- on key-value pair RDDs: reduce **by key**, group **by key**, map values, keys, values, join, subtract by key
## Actions
- collect, count, count by value, take, top, take ordered, take sample, reduce, fold, aggregate, for each  
- on pair RDDs: count by key, lookup

## Physical layer
parallel execution, optimization: avoid expensive network communication, stage.
### Stage
it is a sequence of parallelizable tasks performed on a single machine.

#### Dependency
- narrow dependency: stays on the same machine.
- wide dependency: cannot have a single stage, need to shuffle around. Need the network. 

### General DAG
partition the DAG into sub-graphs, which can be computed without the network communication.

## Performance tuning
- persisting RDDs
- avoiding a wide dependency: pre-partition according to the keys.

## DataFrames
easy to do mutual transformation between **RDD** and **DataFrame**.

# Abbreviations
DAG: directed acyclic graphs  
RDD: resilient distributed dataset