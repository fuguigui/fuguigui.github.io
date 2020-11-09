---
title: Querying
date: 2019-01-26
tags: [big data]
categories: course notes
---



# XML Navigation

## Operator
- slash: ```/```
- descendant axis : ```//```
- attribute axis: ```@attr-name```
- atomization: ```data(...)```
- filter: ```[]```, e.g.```@code[data(.)="CH"]```
- parent abbreviation: ```/..```
- collections: ```collection(...)```  
Use ```.``` to represent the current element

# JSON Navigation
- using ```.``` as the slash in XML.
- ```.countries``` is different from ```.countries[]```  
- using ```$$``` to represent the current element.

# Construction
- String Escaping
	- JSONiq: using the backslash: ```\",\n``` to replace quote, spare space.
	- XQuery: using ```&quot;$#x000a;``` ...
- Booleans
	- JSONiq: ```true, false```
	- XQuery: ```true(), false()```

# Basic Operations
- Atomization: XML only: ```<a>42</a>+1=42+1=43```
- Empty sequence: ```()+1=()```
- Cardinality: ```(3,4)+2```: error
- General Comparison: =,<,>,>=,<=, but all the types must be the same
- Value Comparison: le,lt,eq, ne, ge, gt
- logics: conjunction, disjunction, not
- non-booleans
	- false: ```"", 0, ()```
	- true: ```"foo", 42, (<foo/>,<bar/>,1,"foo")```
# Architecture of a query processing engine
1. Parsing: from query to abstract syntax tree
2. translating: from AST to Expression tree
3. Optimization: Expression Tree
4. Code Generation: from Expression Tree to Iterator Tree

# Stream Execution
