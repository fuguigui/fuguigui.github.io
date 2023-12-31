---
title: BD14 Graph Database
date: 2019-01-27
tags: [System, Big Data, Graph]
categories: [Learning Notes]
---



Graph Databases don't like shards.

# Issues
- if using RDMS: the joins are really expensive, not efficient for relations. We often do **traversal or reverse traversal** in relations.
-  To solve that, we use **index-free adjacency**

# Ingredients
nodes, edges, properties, labels

# Graph databases
- property graph: Neo4j
- triple stores(RDF): subject, property, object.


# Abbreviation
IRI: international resource identifier

# Syntax
## RDF/XML
- subject ```<rdf:Description rdf:about="subject-IRI">```
- property: within the subject tags: ```<rdf:...> <geo:property1> ...</geo:property1></rdf:...>```
- object: within the property tags: ```<geo:property1 rdf:resource="object-IRI"/>``` or ```<geo:property2>object-value<geo:property2>```
## JSON-LD
```{"@id":subject, "rdf:type":subject-type,"property1":"object1","property2":"object2"}```
## Turtle
```@prefix sub:IRI. @prefix object:IRI. @prefix prop:IRI sub:self prop:sub-prop object:sub-obj ...```

# Querying
## Cypher
Format: ```(node1)-[:edge-label1]->(node2)-[:edge-label2]<-(node3)```  
- anchoring a label: ```(node1:label1)```  
- filtering a property: ```(node1 {prop-key: 'prop-value'}```  
- combining:  ```(node1: label1 {prop-key: 'prop-value'}```  
- variable repetition: ```(node1)-[:edge-label1]->(node2)-[:edge-label2]->(node1)```   
- variable length path: ```(alpha)-[*1..4]->(beta)```  
- MATCH clause: ```MATCH (one-query) (WHERE CONDIONS) RETURN gamma```  
- CREATE clause: use , to seperate ```CREATE (),(),()```

## SPARQL

# Architecture
**No shards**  
Master-slave: Master has the entire graph, in the slaves, only have copies. **Data replication** to avoid data loss(synchronization), to improve performance of scalability(everybody gets to connect to master/slave). 

## Write
how to guarantee the consistency?  
- write to the master
- or to a slave. It is blocked until it makes sure that the master is up-to-date.

## Hardware
Fixed-size records: serialize the nodes/edges/labels/properties.   
- properties storage: save key-value pairs  
- relationship storage: 
	- double links for free iteration
	- from an edge view: a pointer to the source(target) node, a pointer to the s/t-previous edge, a pointer to the s/t-next edge.
- typical size:  
	- node: 9 bytes
	- relationship: 33 bytes
	- relationship name: 5 bytes
	- property: 33 bytes

# Further Reading

[Graph Databases, 2nd Edition](https://www.oreilly.com/library/view/graph-databases-2nd/9781491930885/) Chapter 1, 2, 3, 4, 6, 



