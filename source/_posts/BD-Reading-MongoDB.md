---
title: Reading notes of MongoDB
date: 2019-01-22
tags: [big data]
categories: course notes, book reading
---


# Chapter 3 Creating, Updating, and Deleting Documents

## Inserting and Saving Documents
Batch inserts allow you to pass an array of documents to the database. Sending dozens, hundreds, or even thousands of documents at a time can make inserts significantly faster.

Current versions of MongoDB do not accept messages longer than 48 MB
## Insert Validation
- size: all documents must be smaller than 16 MB. To see the BSON size (in bytes) of the document doc, run `Object.bsonsize(doc)` from the shell
- "_id field": check’s the document’s basic structure and adds an "_id" field if one does not exist

## Removing Documents
### `remove`
The `remove` function optionally takes
- a query document as a parameter. 
- **only documents** that match the criteria will be removed.

There is no way to undo the remove or recover deleted documents.

### `drop`
` db.tester.drop()`: the entire collection is dropped. It is much faster.

## Updating
### Document Replacement
when you create a variable from the `find` result, you copy the `_id` to that. If you want to use this variable to update documents, you should avoid creating a duplicate `_id`.

You can use `_id` in update location. like `db.people.update({"_id" : ObjectId("4b2b9f67a1f631733d917a7c")}, joe)`. 

Using `_id` for the criteria will also be faster than querying on random fields, as `_id` is indexed. 

### Using modifiers
to update certain portions of a document.
When using modifiers, 
- the value of `_id` cannot be changed.
- Values for any other key, including other **uniquely indexed keys**, can be modified.

### `$set`
- sets the value of a field. 
- If the field does not yet exist, it will be created. 

! You must always use a `$-modifier` for adding, changing, or removing **keys**, instead of using `update()`. Because `update()` actually does a full-document replacement, if there is no `$-modifiers`.


`unset`:  can remove the key altogether. 
`> db.users.update({"name" : "joe"},

... {"$unset" : {"favorite book" : 1}})`

### `$inc`
- change the value for an existing key or 
- to create a new key 

`$inc` is similar to `$set`, but it is designed for incrementing (and decrementing) **numbers**.  can be used only on values of type integer, long, or double.

### Array Modifiers
#### `$push`: 
- adds elements to the end of an **array** if the array exists 
- creates a new **array** if it does not.


# Chapter 4
# Chapter 5
