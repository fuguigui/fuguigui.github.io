---
title: Syntax
date: 2019-01-26
tags: [System, Big Data]
categories: [Learning Notes]
---

# Use case

- Write-intensive: highly **normalized**: avoid update anomalies!
- Read-intensive: highly **denormalized**: avoid joins!

# Denormalizing road
- relational database: **homogeneous** collection of **flat** items
- document store(**semi-structured**): **heterogeneous** collection of **arborescent** items.

# Syntax
One syntax defines a language. If a sentence is **well-formed**, it belongs to this language. Otherwise, it doesn't.

## XML
### Entities
- element, defined by ```<element-name>...</element-name>``` or ```<element-name/>```
- attribute: 
	- **only** within the brackets, defined like ```<a attr="value"/>```.
	- And **only** attributes can appear inside opening element tag. 
	- two attributes **cannot** have the same name within a single element.
- text: **only** between the element tags, e.g. ```<a>this is text</a>```. **cannot** appear outside of elements.
- comment: ```<!-- This is a comment -->```
- processing instructoin: defined by ```<? ...?>```,e.g.```<?xml vesion="1.0"?>```
- CDATA sections: appeared like the text, the format is ```<![CDATA[ ... ]]>```
- Document Type: ```<!DOCTYPE document[(internal subset)]>```
- (Internal) Entity declarations: ```<!ENTITY name "value">```, using ```&name;``` to get the value.
- **External parsed entities**: conditions are relaxed **???: text at top level, multiple elements.**. Defined method: ```<!ENTITY name SYSTEM "path">```
- **???External unparsed entities**: allows an entity to appear as the attribute's value??
- 

### Entity References
Five: ```&lt;```: <, ```&gt;```: >,  ```&apos;```: ', ```&quot;```: ", ```&amp;```: &,  

### Character References
two formats: hex, beginning with ```&#x``` and dec, beginning with ```$#``` 

### Norms
#### XML valid name
- ```a-z,A-Z, :, _``` are allowed anywhere in a name
- ```0-9, -, .``` are allowed but not at start.
- other ASCII characters are not allowed in XML names. 

#### Whitespaces
whitespace matters!  
space: ```#x20;```; tabs: ```#x9;```; carriage return: ```#xD;```; newlines: ```#xA;```  
Carriage return is automatically replaced with newline before parsing.

if in an element declaring that ```<item xml:space="preserve">```: the space before its sub-elements will not be ignored. By default, such whitespaces are ignored.

#### Namespace
- prefix: anything, can be omitted.
- local name
- namespace, scope: this and within this element
using ```xmlns:prefix-name:namespace``` to define a prefix, and by ```prefix:local-name``` to use QName.  
If the prefix is omitted, it will define a **default namespace** for this element (and its sub-elements).

If no namespace is defined, all the elements are in **no namespace**. That means there is no default namespace without definition.  

Unprefixed attributes are in no namespace even if there is a default namespace in scope.



#### Best practice
- put all namespace bindings in root element
- Try to keep the same bindings across all documents whenever possible!
- use parsimoniously

## JSON
### Elements
- string
- number
- null: Null is invalid.
- boolean: true, false
- array
- object

### Well-formedness
## Others
XHTML, YAML (the "Python of JSON"), CSV(comma separated values)

# Further Reading

[XML in a Nutshell](https://www.oreilly.com/library/view/xml-in-a/0596007647/) Chapter 1, 2, 4, 9

Thanks Alessandro Stolfo for reading notes

{% pdf 10-xml-syntax.pdf %}