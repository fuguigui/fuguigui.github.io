---
title: Wide Column Store
date: 2019-01-24
tags: [big data]
categories: course notes
---



HBase: design to run on a **scalable** cluster of **commodity hardware**, built on **HDFS**. Founding paper: Google's BigTable.

**Design paradigm of Big Table**: store together what is accessed together, because join is really expensive.
# Columnar Model: Denormalized
## Column-oriented stores
also, wide column stores, column-family-oriented stores:   
Column family: must be know **in advance**, but the columns can be added on the fly.
## Queries
Get, Put, **Scan**, Delete

# Physical-level
### Regions
defined by **min-included** rowID and **max-exclued** rowID

### Column Families
stored together, composed as **HFile** on HDFS. 

## HFile
as an SSTable, Only allowed to be written **sequentially** in a single batch.
### Prefix code
keylength+valuelength+key+value

Advantages: without consideration of separation.

How to realize prefix code? Save the key/value-length in a given length of bits
#### Key
row length(fixed bits), row(key), column family length, column family, column qualifier, timestamp (for versioning) , key type (for marking as deleted)

### Versioning
Total order, only integers. How to maintain the total order? HBase guarantees ACID on the **row** level

Different versions of same cell.
#### Blocks
We read many key-values together, which is called Block ( the default size is 64kb). But the size is not fixed. If we have longer value at the end, which size is larger than block size, we **don't split** the block, just having a longer block.

Short summary: Table, into Regions, into Stores, saved into StoreFile, composed of Block
[on the disk](./pic/0501.png)

# Storage
## In Memory
- MemStore: It is the **write** cache. The main role of MemStore is to store new data which has not yet been written to disk.
- LRU BlockCache: it is the **read** cache. The main role of BlockCache is to store the frequently read data in memory. 
- Indices of HFiles
- Bloom Filters

### MemStore
- In simple words, before a permanent write, a write buffer where HBase accumulates data in memory is what we call the MemStore.
- While the MemStore fills up, its contents flush to disk to form an HFile.
- It forms a new file on every flush, rather than writing to an existing HFile.
- Basically, for HBase, the HFile is the underlying storage format.
- Per column family, there is one MemStore. It is possible that one column family can have multiple HFiles, but not vice versa.
### Bloom Filters
quickly to judge whether an item belongs to a set. It uses several hash functions to realize that. It can be false positive.
# Architecture
HMaster RegionServer

## Bootstrap
- **META table** stores the locations of all the regions in HBase.
- **Root table** stores the information of META table. (it is been dropped)
## HMaster
- DDL operations: create table, **not** delete table.
- Assign regions to RegionServers
- Split regions
- handles RegionServer failovers

## Region Server
- the data which we manage by Region Server further stores in the Hadoop DataNode. And, all HBase data is stored in HDFS files.
- Region Servers are collocated with the HDFS DataNodes, which also enable **data locality**. 

# Process
## Writing new cells
HFile(StoreFile) is on the disk.  
1. First write cells into WAL(write-ahead-log, It is a file on the distributed file system. ) in MemStore, 
2. The data to be written is forwarded to MemStore which is actually the RAM of the data node, as soon as the log entry is done. All the data is written in MemStore which is faster than RDBMS
3. then sort these cells into a StoreFile.
4. Further, ACK (Acknowledgement) is sent to the client as a confirmation of task completed, as soon as writing data is completed.

When to flush:  
- reaching max MemStore size in a store  
- reaching overall max MemStore size  
- reaching full Write-Ahead Log

## Reading from a Store
read from **everywhere**! It is efficient, because we have index. Then, take versions.

## Compaction
### Minor Compaction
When we have a lot of store files, we take them, sort again **in memory** and flush back into a single file.
### Major Compaction
- a process of combining the StoreFiles of regions into a single StoreFile, is what we call HBase Major Compaction. 
- Also, it deletes remove and expired versions permanently. 
- The region will split into new regions after compaction, if the new larger StoreFile is greater than a certain size (defined by property).
###  Trees
[principles introduction](https://www.jianshu.com/p/06f9f7f41fdb)
#### B+ Tree
 For classical RDBMS. It supports the range query. To reduce **seek** times.
#### LSM Tree
 Log-structured merge tree. **Transfer efficient**, because you only write the entire file sequentially. It is used to minimize the times we need to merge.

The logs are stored as a small tree in memory. When the tree grows bigger, it will be flushed into the disk. The trees in disk are regularly merged as a bigger one for faster read when the disk does compaction.

# Caching
To read faster, there are 2 levels of caches: level 1: LRU block cache and level 2: bucket cache. Finally, go to HDFS.

when not to use caching?
- Batch processing
- random access
# Best practice
when to use HBase? The number of rows: **millions**: RDBMS; **Billions**: HBase.


# Spanner
tries to bring back ACID to Big Data.
## Features
- Timestamp, showed to users.
- Directory: like regions
- tablet: put regions together.
- 1,000,000,000,000s of rows. **Trillions of** rows, not fit into a single cluster.
- **universemaster** over masters.
- sacrifice higher availability to get **lower latency**.
## Architecture
- 100s of data centers
- 1,000,000s of machines
- multiple data centers.
# Abbreviation
LSM Tree: log-structured merge tree.  
LRU: least recently used.
