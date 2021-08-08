---
title: Data Model
date: 2019-01-22
tags: [System, Big Data]
categories: [Learning Notes]
---

# Type System

edge vs node labeling: labels are on the edges(JSON)/nodes (XML)
## Shared properties
- distinction between atomic types and structured types
- same categories of atomic types
- lists and maps as structured types
- sequence type cardinalities: *(zero or more, repeated),?(zero or one, optional),+(one or more)
### Atomic types
- string
- number
	- arbitrary precision decimales and integers
	- signed and unsigned integer types
	- IEEE 754 standard: float: 32bits, ca 7digits, 10^-37 to 10^37; double: 64bits, ca 15digits, 10^-307 to 10^308
- booleans
- dates and times
	- dates: year+month+day
	- times: hours+minutes+seconds
	- timestamp: date+time
- Time interval
- binaries
- null
### Structured types
- associative arrays( maps) : JSON object, set of XML attributes
- ordered lists: JSON array, XML element

# Validation
pipeline: document->well-formedness->validation

PSVI: post-schema-validation infoset: infoset + **types**
## DTD validation
### Element type declaration
- empty content: ```<!ELEMENT name EMPTY>```
- simple content: only includes text, using ```#PCDATA```, ```<!ELEMENT name (#PCDATA)>```
- complex content: meaning including other elements: ```<!ELEMENT name (sub1/+/*/?,sub2, ...)>```, the more advanced:```<!ELEMENT name (bar*|(foo|foobar)+)? >```
- mixed content: includes both other elements and text, e.g. ```<!ELEMENT name (#PCDATA|foo)*>```, or can also use ANY ```<!ELEMENT name ANY>```
### Attribute-List Declaration
Format: ```<!ATTLIST ele-name attr-name ATTR-FORMAT CARDINALITY>```
- attribute format: CDATA, NMTOKEN(s), ID(REFS)
- cardinality: 
	- #REQUIRED: must have
	- #IMPLIED: can have
	- "string": the value of the attribute, can be omitted.
	- #FIXED "string": must written out, cannot be omitted.

# XML Schema
Two files: .xsd: the schema defined file and .xml: the instance file
- .xsd: the definition of an element: ```<xs:element name="ele-name", type="xs:string/xs:integer/user-defined type..."/>```
- .xml: within the defined element: using ```<ele-name xmlns:xsi="..." xsi:schema-location="schema-name.xsd">```

## Element types
### Simply types: built-in
- strings: string, anyURI, QName; 
- numbers: decimal, float, double, integer, ...; 
- booleans: boolean
- dates and times: dateTime, time, date, gYearMonth, gMonthDay, gYear, gMonth, gDay, dateTimeStamp
- time intervals: duration, yearMonthDuration, dayTimeDuration
- Binaries: hexBinary, base64Binary
- NUll
### User-defined types
#### Simple types
- restriction: ```<xs:restriction base="built-in type"> <xs:length value="3">...</xs:restriction>```
- list: ```<xs:list itemType="xs:string"/>```, instance: ```<foo>foo bar foobar</foo>```
- union: ```<xs:union memberTypes="xs:integer xs:boolean"/>```, instance: ```<foo>true<foo>```
#### Complex types
- **empty**: ```<xs:complexType name="emptyType">```
- simple content **??? what is different with the simple type?**: 
- complex content: can include sub-elements
- mixed conent: can include both text and sub-elements. ```<xs:complexType, name="..." mixed="true">```
#### Keys
XML allows you to make an attribute as a key of the element. the definition is 
```<xs:element ...>....<xs:key name="key-name"> <xs:selector xpath="the-aim-element"/><xs:field xpath="@the-key-attribute"/></xs:key></xs:element>```

How to use? 1. simple type of attributes; 2. named types; 3. anonymous types: defined where to use, without being named.

## Schema Location
### without namespace
in the instance file:  ```<xsi:noNamespaceSchemaLocation="schema.xsd">```. Correspondingly, in the schema file, there is no definition on the namespace.
### With namespace
in the instance file:  ```<xsi:SchemaLocation="namespace/schema.xsd">```.
In the schema file, ```<xs:schema xmlns:xs="..." targetNamespace="namespace" ...>``` 

## Import Schema
in the .xsd file, we can use types defined in other schema files, by using 	```<xs:import namespace="other-schema-namespace" schemaLocation="other-schema-name.xsd"/>```. To import, we use ```ref```, in the sentence: ```<xs:element ref="prefix:defined-type" ...>```

# Other technologies
Avro: It is a language that allows you to define schema, an apache language.   
Process: 
1. read the language-independent schema
2. compilation the schema and put it in somewhere of the code
3. read in the data according to the schema.

XDM: XPath and XQuery Data Model, JDM: JSONiq Data Model

XDM: seven kinds of XML Nodes: Document Node, Element node, Attribute node, Text Node, Comment node, Processing instruction node, namespace node.
# Abbreviation
PSVI: post-schema-validation infoset  
DTD: document type declaration  
XDM: XPath and XQuery Data Model  
JDM: JSONiq Data Model

# Further Reading

[XML in a Nutshell](https://www.oreilly.com/library/view/xml-in-a/0596007647/) Chapter 3, 17

Thanks Ruben and Christina for reading notes

{% pdf 11_DataModels.pdf %}