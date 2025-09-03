---
title: MongoDB CRUD - Updating a Document by Replacing the Entire Document (No Update Operators)

---

***

# MongoDB CRUD : Updating a Document by Replacing the Entire Document (No Update Operators)

Welcome back to the MongoDB learning series! This post continues the deep dive into CRUD operations as part of our preparation for the MongoDB Associate Developer exam.

Today, we cover the important concept of **updating a document by fully replacing it** using the `updateOne` or `replaceOne` commands *without* using update operators like `$set`. Understanding this nuanced operation is crucial for both passing the exam and writing safe production code.

***

## Understanding the Scenario: Full Document Replacement

- Unlike updating specific fields, this operation **replaces the entire existing document** (except `_id`) with the new one you provide.
- Any fields missing in the new document **will be removed** from the stored document.
- This is powerful but risky: unintended data loss can occur if you accidentally omit fields.

***

## The Basic Syntax

```javascript
db.collection.replaceOne(
  <filter>,
  <replacementDocument>
)
```

**Or equivalently:**

```javascript
db.collection.updateOne(
  <filter>,
  <replacementDocument>
)
```

- `<filter>`: The query to find the document(s) to replace.
- `<replacementDocument>`: The new document body *replacing* the older one (must include all desired fields).

***

## Hands-On Example Using Sample Dataset (`sample_mflix.movies`)

### Step 1: Find Original Document

```javascript
db.movies.findOne({ title: "Inception" })
```

*Sample original output:*

```json
{
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "title": "Inception",
  "year": 2010,
  "genres": ["Action", "Sci-Fi"],
  "directors": ["Christopher Nolan"],
  "runtime": 148,
  "released": ISODate("2010-07-16T00:00:00Z"),
  "awards": {
    "wins": 4,
    "nominations": 10,
    "text": "Won 4 Oscars"
  }
}
```

***

### Step 2: Replace Document with a New One

```javascript
db.movies.replaceOne(
  { title: "Inception" },
  {
    title: "Inception: Director's Cut",
    year: 2010,
    genres: ["Action", "Thriller"]
    // Note: 'directors', 'runtime', 'released', 'awards' omitted intentionally
  }
)
```

***

### Step 3: Result

```javascript
db.movies.findOne({ title: "Inception: Director's Cut" })
```

*Expected output:*

```json
{
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "title": "Inception: Director's Cut",
  "year": 2010,
  "genres": ["Action", "Thriller"]
  // All previously stored fields that were NOT in the replacement document are now removed!
}
```

***

## Explanation of Key Fields and Behavior

| Field           | Explanation                                           |
| --------------- | ---------------------------------------------------- |
| `_id`           | Preserved automatically; you do **not** replace `_id`|
| `title`         | Updated (replaced) to new value                       |
| `year`          | Preserved as provided                                 |
| `genres`        | Updated; note array change from original              |
| Other fields    | Since omitted (directors, runtime, awards), now gone |

**Important:** When using full replacement, the replaced document **becomes exactly as the new document specified — all fields not present are deleted**.

***

## Why Use Full Replacement?

- When you need to **completely overwrite** a document with a new one, for example restoring from a backup.
- Use with caution in general; safer to use update operators for partial changes.

***

## PyMongo Equivalent

```python
from pymongo import MongoClient

client = MongoClient("mongodb://localhost:27017/")
db = client.sample_mflix

replacement_doc = {
  "title": "Inception: Director's Cut",
  "year": 2010,
  "genres": ["Action", "Thriller"]
  # 'directors', 'runtime', and other fields omitted
}

db.movies.replace_one({"title": "Inception"}, replacement_doc)
```

***

## Key Points for Exam and Interviews

- Using `replaceOne()` or `updateOne()` without any update operators does a **full document replacement**.
- All fields **not present** in the replacement document are **removed** from the original.
- `_id` field is never replaced or removed.
- For **partial updates**, always use update operators like `$set`.
- Mistakes in replacement documents may inadvertently delete critical data.

***

## Practice Questions (No Answers)

1. What happens to fields that are omitted in a `replaceOne` operation?  
a) They remain unchanged  
b) They are deleted from the document  
c) The operation fails  

2. Which update command would you use to **only modify certain fields without removing others**?  
a) `replaceOne()` with partial document  
b) `updateOne()` with `$set` operator  
c) `updateOne()` with full document replacement  

3. Given a document with fields `{a:1, b:2, c:3}`, what is the result after:  
```javascript
db.collection.updateOne({a:1}, {b:5})
```
a) Document now has fields `{b:5}`  
b) Document now has `{a:1, b:5, c:3}`  
c) Operation error  

***

## References

- MongoDB Official Docs — [replaceOne](https://www.mongodb.com/docs/manual/reference/method/db.collection.replaceOne/)  
- MongoDB Official Docs — [updateOne](https://www.mongodb.com/docs/manual/reference/method/db.collection.updateOne/)  
- MongoDB Official Docs — [Update Operators](https://www.mongodb.com/docs/manual/reference/operator/update/)  
- [Previous Blog on Insert One & Insert Many](#)  

***
