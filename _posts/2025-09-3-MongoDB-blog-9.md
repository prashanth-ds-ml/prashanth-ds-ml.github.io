---
title: MongoDB CRUD —  Using Upsert to Update or Insert Documents
---

***

# MongoDB CRUD — Using Upsert to Update or Insert Documents

Welcome back to the MongoDB learning series! Today, we explore **upsert operations**, a flexible and powerful way to update documents and insert them if they don’t exist.

Upsert is a vital concept for the MongoDB Associate Developer Exam and practical data management tasks.

***

## What Is Upsert?

- An **upsert** combines update and insert functionality.
- If the document matching the filter exists, it is updated.
- If it does not exist, a new document is inserted based on the update criteria.
- This avoids the need for separate existence checks before updates.

***

## Syntax

```javascript
db.<collection>.updateOne(
  <filter>,
  <update>,
  { upsert: true }
)
```

or for multiple documents:

```javascript
db.<collection>.updateMany(
  <filter>,
  <update>,
  { upsert: true }
)
```

- `<filter>`: Query to match existing documents.
- `<update>`: Update operators like `$set`.
- `upsert: true`: Flag to enable insert if no match found.

***

## Hands-On Example Using `sample_mflix.movies`

Suppose we want to add or update a movie's rating, inserting the document if it doesn’t exist.

```javascript
db.movies.updateOne(
  { title: "The Matrix" },
  { $set: { rating: 8.7, year: 1999 } },
  { upsert: true }
)
```

***

### Scenario 1: Document Exists

If `"The Matrix"` exists, it will update its `rating` and `year` fields.

***

### Scenario 2: Document Does Not Exist

If `"The Matrix"` is not found, it inserts a new document similar to:

```json
{
  "title": "The Matrix",
  "rating": 8.7,
  "year": 1999
}
```

MongoDB automatically generates `_id`.

***

## Explanation Field-by-Field

| Element       | Description                                             |
| ------------- | -------------------------------------------------------|
| `filter`      | Finds documents with `title: "The Matrix"`             |
| `$set`        | Updates or adds `rating` and `year` fields              |
| `upsert: true`| Ensures insert if no match (upsert enabled)             |
| Result       | Existing doc updated, or new doc inserted               |

***

## PyMongo Equivalent

```python
from pymongo import MongoClient

client = MongoClient("mongodb://localhost:27017/")
db = client.sample_mflix

db.movies.update_one(
    {"title": "The Matrix"},
    {"$set": {"rating": 8.7, "year": 1999}},
    upsert=True
)
```

***

## Key Points for Exam & Interviews

- The `upsert` option in `updateOne`/`updateMany` enables insert if no document matches the filter.
- Upsert combines update and insert into one atomic operation.
- If inserting, MongoDB uses the update document combined with the filter to create the new document.
- `_id` is auto-generated if not explicitly provided in the upsert insert.
- Upsert avoids race conditions by atomic checking and insert/update in one command.
- Common exam focus due to its practical significance in application logic.

***

## Practice Questions (No Answers)

1. What does the `upsert` option do in an update command?  
   a) Updates multiple documents only  
   b) Inserts a new document if no matching document found  
   c) Deletes the matched document

2. What fields will be present in a document inserted by this upsert command?  
   ```js
   db.movies.updateOne(
     { title: "New Movie" },
     { $set: { year: 2025, rating: 7.5 } },
     { upsert: true }
   )
   ```
   a) Only `year` and `rating`  
   b) `title`, `year`, and `rating`  
   c) Only `title`

3. Which of the following statements about upsert is FALSE?  
   a) Upsert operations can be performed with `updateOne` or `updateMany`  
   b) The existing document is replaced entirely on upsert  
   c) Upsert will insert a new document if none match the filter  

***

## References

- [MongoDB Docs — Update with Upsert](https://www.mongodb.com/docs/manual/reference/method/db.collection.updateOne/#upsert-parameter)  
- Previous Blog on `$set`: [Blog 8 — Updating with $set](#)  

***
