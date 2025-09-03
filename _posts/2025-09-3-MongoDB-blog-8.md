---
title: MongoDB CRUD —  Updating Documents Using the `$set` Operator
---

***

# MongoDB CRUD —  Updating Documents Using the `$set` Operator

Welcome back to the MongoDB learning series! Continuing our journey through CRUD operations for the MongoDB Associate Developer Exam, today we explore **how to update specific fields in documents using the `$set` operator**.

This is one of the most commonly used update operations in MongoDB and crucial for modifying data safely without overwriting the entire document.

***

## Understanding the Scenario: Partial Updates with `$set`

- Instead of replacing the entire document, `$set` updates *only the specified fields*.
- Fields not mentioned remain unchanged.
- New fields can also be added by specifying them in `$set`.

***

## Syntax

```javascript
db.<collection>.updateOne(
  <filter>,
  { $set: { <field1>: <value1>, <field2>: <value2>, ... } }
)
```

Or, to update multiple documents:

```javascript
db.<collection>.updateMany(
  <filter>,
  { $set: { <field1>: <value1>, ... } }
)
```

- `<filter>`: Criteria to match documents.
- `$set`: Update operator specifying fields and their new values.

***

## Hands-On Example Using Sample Dataset (`sample_mflix.movies`)

### Existing Document

```json
{
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "title": "Inception",
  "year": 2010,
  "genres": ["Action", "Sci-Fi"],
  "runtime": 148
}
```

### Update Command - Modify Runtime and Add Rating

```javascript
db.movies.updateOne(
  { title: "Inception" },
  { $set: { runtime: 150, rating: 8.8 } }
)
```

### Resulting Document

```json
{
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "title": "Inception",
  "year": 2010,
  "genres": ["Action", "Sci-Fi"],
  "runtime": 150,
  "rating": 8.8
}
```

- Notice `runtime` updated from 148 to 150.
- New field `rating` added.
- All other fields untouched.

***

## Explanation

| Concerned Field | Original Value      | Updated Value | Explanation                       |
|-----------------|--------------------|---------------|---------------------------------|
| `runtime`       | 148                | 150           | Numeric field updated            |
| `rating`        | (not present)      | 8.8           | New field added                  |
| Others          | (unchanged)        | (unchanged)   | Remain intact                   |

This granular update is essential for precise control and data integrity.

***

## PyMongo Equivalent

```python
from pymongo import MongoClient

client = MongoClient("mongodb://localhost:27017/")
db = client.sample_mflix

db.movies.update_one(
    {"title": "Inception"},
    {"$set": {"runtime": 150, "rating": 8.8}}
)
```

***

## Key Points for Exam & Interviews

- `$set` only changes specified fields; other fields remain the same.
- Avoids accidental data loss seen in full document replacement.
- You can add new fields not previously present.
- Supports updating multiple fields simultaneously.
- `$set` is a fundamental update operator in MongoDB queries and handling real-world updates safely.
- Essential knowledge for certification exams and backend developer roles.

***

## Practice Questions (No Answers)

1. Which MongoDB command updates only specified fields without affecting others?  
a) updateOne with full document replacement  
b) updateOne using `$set`  
c) insertOne  

2. After executing:  
```javascript
db.movies.updateOne({title: "Inception"}, {$set: {year: 2012, rating: 9.0}})
```
What will be true?  
a) Only `year` and `rating` fields are updated or added  
b) All other fields in the document are deleted  
c) The operation fails  

3. What happens if you try to update a field using `$set` that did not previously exist in the document?  
a) MongoDB throws an error  
b) MongoDB creates the new field with the given value  
c) MongoDB ignores the new field  

***

## References

- MongoDB Official Docs: [Update Operators - $set](https://www.mongodb.com/docs/manual/reference/operator/update/set/)  
- Previous Blog: [MongoDB Update – Full Document Replacement](#)

***
