---
title: Big Data course notes
date: 2019-03-01
tags: [System, Big Data]
categories: [Sharing]
top: true
---

I took the course Big Data in the autumen semester 2018, opened by [Dr. Ghislain Fourny](http://people.inf.ethz.ch/gfourny/), Department Information ETH Zurich. 

# Content

From the course introduction

>This course gives an overview of database technologies and of the most  important database design principles that lay the foundations of the Big Data universe. The material is organized along three axes: data in the  large, data in the small, data in the very small.  A broad range of  aspects is covered with a focus on how they fit all together in the big  picture of the Big Data ecosystem.

- physical storage: distributed file systems *HDFS*, object storage *S3*, key - value stores
- logical storage: document stores *MongoDB*, column stores *HBase*, graph databases *neo4j*, data warehouses *ROLAP*
- data formats and syntaxes *XML, JSON, RDF, Turtle, CSV, XBRL, YAML, protocol buffers, Avro*
- data shapes and models: tables, trees, graphs, cubes
- type systems and schemas: atomic types, structured types :arrays, maps, set - based type systems  ?, * , +
- an overview of functional, declarative programming languages across data shapes *SQL, XQuery, JSONiq, Cypher, MDX*
- the most important query paradigms: selection, projection, joining, grouping, ordering, windowing
- paradigms for parallel processing, two-stage *MapReduce* and DAG-based *Spark*
- resource management *YARN*
- what a data center is made of and why it matters: racks, nodes, ...
- underlying architectures: internal machinery of HDFS, HBase, Spark, neo4j
- optimization techniques: functional and declarative paradigms, query plans, rewrites, indexing
- applications.



I happened to found all the course videos are avaiable on [YouTube](https://www.youtube.com/watch?v=4t6BR_fzLR4&list=PLs5KPrcFtb0UHTl_gXR_EYW28m9pD8iYN). Take it for free!

# Experience

From my own experience, this course is very system-style. I didn't have a strong background on this and made a lot of efforts to learn this course. The lecturer gives many recommended readings almost for each lesson. Reading them benefited me a lot while reading all of them was impossible :joy:. Fortunately, the contents are not very difficult to understant but the course volume is quite big. Almost every week we were exposed to a different technology with many tools, regulations, applications and details. The final exam was more like an Encyclopedia knowledge contest, full of knowledge. Each piece of knowledge was not hard to memorize but the point was there were too much! Frankly speaking, this course is very useful in the practical as well as industrial fields. I have learnt a lot from it. 

I gonna share my course notes here as well as extra reading notes. Enjoy!

# Notes Content

[Introduction](../BD-1Intro/index.html)

[Database Basics](../BD-2DBBasics/index.html)

 [Storage](../BD-3Storage/index.html)

[File System](../BD-4FileSystem/index.html)

[Wide Columne Store](../BD-5ColumnStore/index.html)

[MapReduce](../BD-6MapReduce/index.html)

[YARN](../BD-7YARN/index.html)

[Spark](../BD-8Spark/index.html)

[Performance at large scale](../BD-9PerformancLargeScale/index.html)

[Syntax](../BD-10Syntax/index.html)

[Data Model](../BD-11DataModel/index.html)

[Querying](../BD-12Querying/index.html)

[Document Stores](../BD-13DocumentStores/index.html)

[Data Warehousing](../BD-15Datawarehousing/index.html)