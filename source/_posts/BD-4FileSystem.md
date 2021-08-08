---
title: File system
date: 2019-01-24
tags: [System, Big Data]
categories: [Learning Notes]
---

# Use Cases

- **Billions** of **TB** files: the files are relatively small, but the amount is large  
	- Object Storage (technology)
	- Key-value Model(model)
- **Millions** of **PB** files: the files are large, but the amount is relatively small.
	- block(file) storage (technology)
	- file system (model)

# GoogleFS (GFS)
 It is the first DFS. It says there are some characteristic and requirements to be realized to design a DFS, which include: fault tolerance, file update model, performance requirements.
## Design a DFS
### Fault tolerance
Even we know the nodes will fail, the system should continue to work. What to do to realize fault tolerance?
1. Monitoring
2. Error Detection
3. Automatic Recovery
### File update model
There are two typical models can be used: random access and upsert/append only.  
For **GB** size files, we choose **append**: immutable, not that flexible.
### Performance requirements
Top priority **throughput**, secondary priority: latency.

# The Model of DFS
## Logical Layer
you can choose key-value model or file system
## Physical Storage
Object Storage or Block Storage. The latter, what the hard driver actually does. Files are not stored continuously, but in blocks and achieved as blocks. The hard driver read blocks, not single bits. 

# HDFS
H means Hadoop. Initiated in 2006. Inspired by GFS(2003),  MapReduce (2004), and BigTable (2006).
Its primary features: 1. DFS, 2. MapReduce, 3. Wide Column Store(HBase).

## Blocks
The reason to choose block:
1. Files bigger than a disk
2. simpler level of abstraction

### Size
- The block in a single file system: **4kb**
- The block in Relational Database: **4KB-32KB**
- The block in DFS: **64MB-128MB**

## Architecture
Master-Slave model.
- Master: Namenode
- Slave: Datanode

From a file view: the file is splited into 128MB chunks. Each chunk is replicated for several times. The chunks are stored in datanodes and the namenode knows where one part of a file is stored. If several clients want to read this file, they request to the namenode, and the namenode will return the files' location from the datanodes, and the clients go to the corresponding datanode to read. 

### NameNode
#### Functions: all system-side activity
1. file namespace + access control
2. file to block mapping
3. block locations
#### Compositions
- Memory: 1. file system hierarchy, 2. file to block mapping, 3. block locations
- Persistent Storage: namespace file + edit log
### DataNode
Blocks are stored in datanodes, on the local disk. The datanodes know their own hardware, so they can deal with disk failure. Blocks are identified by **BlockID(64bits)**, 

## Communication
### RPC
#### Client Protocol
The **client** send **Metadata operations** to a **namenode**, and the **namenode** return the **DataNode location and Block IDs**.
#### DataNode Protocol
The **DataNode**(who always **initiate** connection) register on a **NameNode**. Every **3s**, it sends heartbeat to the namenode; Every **6h**, it sends Block Report to the namenode, as well as "BlockReceived" message.  
The **NameNode** sends **Block operations** to the DataNode.
#### DataTransfer Protocol
Between **Client** and **DataNodes**. They transfer data blocks with each other, streaming.  
If a client is **writing** data, it will write on one DataNode, and this DataNode replicate the blocks to other DataNodes, using a pipeline.

### Metadata 
#### Physical level
In each block, there are two files: metadata file and the data file.  
Metadata includes checksum, generation stamp and so on.  
- checksum: is calculated when the block is written, and is used to check for data integrity (may be caused by disk errors, network faults, buggy software and so on) when the file is **read** back.
- generation stamp prevents reading stale data.

#### functionality
- create/delete directory
- write/append to/read/delete file

### Read a file
1. client -> namenode: asks for a file
2. namenode -> client: the block locations, multiple datanodes for each block, sorted by distance.
3. datanodes -> client: form an input stream for client to read.

### Write a file
1. client -> namenode: create command.
2. A circuit
	1. namenode -> client: datanodes for first block.
	2. client -> datanodes: Client creates a pipeline for streaming data to DataNodes.
	3. client -> datanode, datanode->datanode: the client by DataStreamer sends a packet to DN1, DN1 streams to DN2 the same way, and so on. If one node fails the pipeline is recreated with remaining nodes.
	4. datanodes -> client: Ack message
	5. Repeat from the beginning: Namenode -> client: datanodes for second, third ... nth block.
3. client -> NameNode: close/release lock.
4. DataNodes -> NN: through DataNode Protocol, the NN checks for minimal replication.
5. NN -> client: Ack.

## Replicas
the number of replicas: default 3.

Considerations: reliability, read/write bandwidth, block distribution

**Distance**: three layers: node, rack, cluster, 1 between them. e.g. from a node to another node on the same/different rack: 2/4.

### Placement
1. the same **node** as the client (or random), rack A
2. a node in a different **rack**, rack B
3. a different node in the same **rack** with 2, rack B
4. 4 or beyond: random, but if possible: at most one replica per node/at most two replicas per rack. 
Put on different rack is to make the distribution more evenly.

## Performance and Availability
The NameNode is a single node, when it fails, HDFS uses Startup again to solve the problem.
### Startup
It usually takes **30 minutes**.
1. NN read in the persistent storage, and builds the new file system, according to **Namespace life and edit log**.
2. The DNs report to the NN, to recover the **Block locations**.
### Other strategies
1. Checkpoints with secondary NN: SNN composes the old namespace file and edit log as a new namespace file, which will reduces the time in step 1.
2. High Availability(HA): Backup Namenodes: maintains the mappings and locations in memory like the namenode, ready to take over it at all times.
3. Federated DFS: different sub-directories are organized by different NN, more like the file system.

# GFS vs HDFS

|HDFS|GFS|
|----|---|
|NameNode|Master|
|DataNode|ChunkServer|
|Block|Chunk|
|FS Image|Checkpoint image|
|Edit log|Operation log|

Block size: Cloudera HDFS: 128 MB, GFS/Apache HDFS: 64 MB.

# Abbreviations
DFS

# Further Reading

[Hadoop: The Definitive Guide 4th ed.](https://www.oreilly.com/library/view/hadoop-the-definitive/9781491901687/) Chapter 3

[The Hadoop Distributed File System](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwjJotjt6J7yAhUKgP0HHX3RByMQFnoECAQQAw&url=http%3A%2F%2Fstorageconference.us%2F2010%2FPapers%2FMSST%2FShvachko.pdf&usg=AOvVaw0_YA92FFHsSjh4za3pmHae)

