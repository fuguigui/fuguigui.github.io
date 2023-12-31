---
title: BD3 Storage
date: 2019-01-23
tags: [System, Big Data]
categories: [Learning Notes]
---



# Old Local File System
File = Content + Metadata
1. fixed metadata -> fixed Schema
2. organized in a hierarchy
3. files are stored in blocks
4. work in local machine, LAN(local-area network), NAS (network-attached storage), **not in WAN** (wide-area network)

# Object Storage
1. "Black-box" objects
2. Flat and global key-value model
3. Flexible metadata
4. Commodity hardware

# Data Center
1000-100,000 machines in a data center.
RU: rack units
## Per Server
1. 1-14 TB local storage
2. 16GB-4TB RAM
3. 1-10 GB/s network bandwidth
4. 1-100 cores
## Rack modular
- servers  
- storage  
- routers
# Replication
it is for fault tolerance. We have replications in different regions. Why?  
- optimize latency
- resilient to natural catastrophes

# Amazon S3
This is a key/value model. Key is composed:  
   Bucket ID + Object ID.  

- Scalability: The max of an object is 5 TB. An account can have 100 buckets (more upon request). 
- Durability: loss of 1 in 10^11 objects in a year.
- Availability: 99.99% down 1h/year (SLA: 99%-->4days/year, 99.9%-->9 hours/year, 99.99999%-->4 seconds/year), response time <**10 ms** in 99.9% of the cases

# REST APIs
HTTP protocol: Tim Berners-Lee.  
REST (**Representational state transfer**) is the HTTP done the right way. How to do the right way?  
## URI
universal resource identifier: some strings to identify resources. Two sub-kinds: URL(L:located, tell you how to get the resource) and URN (not to tell how)  
e.g. ```http://www.mywebsite.ch/api/collection/foo/object/bar?id=foobar#head```  
scheme + authority + path + query + fragment

## HTTP Methods
- GET: side-effects free
- PUT
- DELETE
- POST: when you want to transfer something really complex.

## Status Code
[REST API tutorial](https://restapitutorial.com/httpstatuscodes.html)

### 1: infromational

### 2: success
- 200: OK, 
- 201: created,  
- 202: Accepted: The request has been accepted for processing, but the processing has not been completed.  
- 204: No Content: the server has successfully processed the request, but not returning any content.
### 3: Redirection
- 300: Multiple choices, 
- 301: Move permanently: This and all future requests should be directed to the given URI.
- 303: See Other: The response to the request can be found under another URI using a GET method. The server has received the data and the redirect should be issued with a separate GET message.
### 4: Client Error
- 400: Bad Request: The request cannot be fulfilled due to bad syntax. General error when fulfilling the request would cause an invalid state. Domain validation errors, missing data, etc. are some examples.  
- 401: unauthorized:  Error code response for missing or invalid authentication token.
- 403: Forbidden: The request was a legal request, but the server is refusing to respond to it. Unlike a 401 Unauthorized response, authenticating will make no difference.  
- 404: Not Found: The requested resource could not be found but may be available again in the future. Subsequent requests by the client are permissible. Used when the requested resource is not found, whether it doesn't exist or if **there was a 401 or 403** that, for security reasons, the service wants to mask.
### 5: Server Error
- 500: Internal Server Error: The server encountered an unexpected condition which prevented it from fulfilling the request.
## With S3
### URI
- Buckets: ```http://bucket.s3(-region).amazonaws.com```
- Objects: ```http://bucket.s3(-region).amazonaws.com/object-name```
### Methods
PUT bucket/object; GET bucket/object; DELETE bucket/object

Is S3 a file system?  
Yes or No. Because 
- in the physical level: the name e.g. "/fruit/apple/red/" is only object ID, instead of actual path. You only have individual objects.
- in the logical level: it works like a file system.

# Microsoft Azure Blob Storage
## Comparison with S3

| .|S3|Azure|
|---|----|----|
|Object ID|Bucket+Object|Account+Partition+Object|
|Object API|Blackbox|Blocks or pages|
|Limit| 5TB|195GB(blocks) 1TB(pages)|

## Storage Replication
intra-stamp replication: synchronous, in the same stream layer  
inter-stamp replication: asynchronous, between two partition layers
## Location Services
1. First key goes to DNS with Account name to get the virtual IP  
2. Then, take the partition+Object keys to the given IP to get the object.


# Key-Value store
It is different from Key-Value Model, which is in the logical layer. Key-value store actaully means smaller objects and low latency.

S3 has a large latency, 100-300ms, but a typical database's latency is 1-9ms. So, we want to make key-object storage work like a database. One realization is DynamoDB:
- smaller objects than S3 (5TB max), only 400KB 
- No metadata. In S3, you can associate metadata with your object, but not in the DynamoDB.

## Basic API
- ```get(key)```: you get the value
- ```put(key, other value)```

### In DynamoDB API
- ```get(key)```: you get the value and **the context**
- ```put(key, context, other value)```: you should give the context in put operation.

Key-value store is a simplification of relational-database:
- simplicity VS more features
- eventual consistency (can have partition tolerance) VS consistency
- performance is much better VS overhead
- scalability VS monolithic( only on a single machine)

## Distributed Hash Table:
### logical ring
1. Each node takes an ID randomly
2. Nodes are logically placed on the ring.
3. ID stored at the next node (clockwise)
4. Adding/removing nodes: only doing with three nodes.
5. Nodes Failure: using duplication in 2(N) ranges.
6. Searching for a key-value: using finger tables, like binary search.

### Pros
1. highly scalable (to the feature 4)
2. robust against failure (to the feature 5)
3. self organizing (cause picking random numbers)
### Cons
1. lookup, not searching.
2. data integrity
3. security issues

### Tokens
We use **virtual nodes, which are called tokens**, to solve the issue: heterogenous performance.

## Vector clocks 
(**the main point**)
###  Purposes of using vector clocks in distributed systems
- Generating a partial ordering of events
- Detecting causality violations between events.
- **Not for** Keeping different versions of objects.
it has the context. 
### mathematical property: 
it is partial ordered:  
1. reflexibility: x - x  
2. transitivity
3. asymmetric 

The format of context ```[(coordinatorID, OrderID),(coordinatorID2,...),...]```

The coordinator ID: each time, a get/put operation has a coordinator to receive and replicate to all the other nodes.

Building a key-value store on the top of peer to peer networks.  
Get operation:```get(key)``` return the value and the context. The coordinator collects all the other nodes values and compare them/ merge them to return. If the contexts are not comparable, it will return several values/contexts.    
Put operation: ```put(key,context,value)```: given the key, the context and the new value.  
Synchronization: remove all the past same values, keep the uncomparable values.  
If the network is partitioned, the system will change the coordinator.

## Merkle Trees
It offers an easy way to see where there is a conflict. The leaf nodes are the hash of data blocks and the non-leaf nodes are the hash of all of its children.


# Design Principles
- incremental stability: can add/delete a node
- symmetry: decentralization
- heterogeneity

# How to scale out?
- simplify the model
- buy cheap hardware
- remove schemas


# Abbrevations
SLA: service-level-agreement
- RDBMS: relational database 

# Further reading notes

Thanks my classmate Claudio Andrea Ferrai and  Ruben Marias for reading and organizing the notes.

{% pdf 03_object_and_keyvalue_storage.pdf %}

