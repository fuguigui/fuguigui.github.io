---
title: Map reduce
date: 2019-01-25
tags: [courses, database, big data]
categories: Big Data
---



# MapReduce

## Process
Input data(key-value pairs)  -> split -> Map -> shuffle -> Intermediate data(key-value pairs) ->  reduce -> output data (key-value pairs)
### Shuffle
1. put all together
2. sort by key
3. partition
## Data types
it allows: (key type1, value type1)->(key typeI, value typeI)->(key typeA,value typeA)  
most often: (key type1, value type1)->(key **typeA**, value typeA)->(key **typeA**,value typeA)  

## Input/output format
table/files
- files: text->KeyValue->SequenceFile: Hadoop binary format, stores generic key-values (```(keylength, Key, ValueLength, Value)```
### InputFormat class
- Table: DBInput Format (RDBMS), TableInputFormat(HBase);
- FileInputFormat
	- KeyValueTextInputFormat (key-value file)
	- SequenceFileInputFormat (Sequence file)
	- TextInputFormat
	- FixedLengthInputFormat (Text)
	- NLineInputFormat
### OutputFormat Class
- Table: DBOutput Format (RDBMS), TableOutputFormat(HBase);
- FileOutputFormat
	- SequenceFileOutputFormat (Sequence file)
	- TextOutputFormat
	- MapFileOutputFormat
## Optimization
### Combine
between map and shuffle. It reduces the amount of shuffling. Combine works on 90% cases. Usually, it is **identical** to reduce function. The **identical** conditions:  
1. Key/Value types must be identical for reduce input and output.  
2. Reduce function must be **commutative** and **associative**

## The Physical Layer
Possible **storage layer**: Local Filesystem, HDFS, S3, Azure Blob storage  
Numbers: several **TBs** of data, **1000s** of nodes  
### Infrastructure (version 1)
Namenode+JobTracker -> DataNode+TaskTracker: which brings query to data. (Task: Map or Reduce)

### Splits
InputSplit is only a **logical** concept. An InputSplit may be stored in different blocks on the HDFS.

1 split = 1 map mask  
In the physical layer: 1 split = 1 block (128MB), but in the approximation. One DN may have multiple splits to perform.   
A split is a set of key-value. A block has the exact size, no matter with the content. That means, we may have a key-value pair cut into two blocks, maybe on different machines. **Fine-tuning** to adjust splits to blocks.

Version 1 and version 2 are two things.

### Shuffling
can start even before the map phase has finished.

### Reducing
A reducer starts a new reduce task when the next key in the sorted input data is different than the previous.

**reducer** is different from the **reduce task**. The user decide the number of reducers. Each partition is sent to a reducer. 

## Issues
1. tight coupling: co-located
2. scalability: only one JobTracker.

## Data-locality
Data locality in MapReduce refers to the ability to move the computation close to where the actual data resides on the node, instead of moving large data to computation. 

How to solve this? See the next chapter.