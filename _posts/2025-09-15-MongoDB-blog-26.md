---
title: MongoDB Advanced CRUD Commands and Cursor Operations 
---

# Introduction

This blog begins a series on advanced MongoDB CRUD commands and cursor operations, essential for the MongoDB Associate Developer exam. It covers `$unset` for removing fields, the positional `$` operator to update specific array elements, atomic arithmetic update operators like `$inc` and `$mul`, and the `findOneAndDelete` command that atomically deletes and returns a document. Mastery of these commands enables precise, safe, and efficient data modifications in MongoDB applications.

# Syntax Breakdown

### 1. $unset Operator — Remove Fields from Documents

```javascript
db.collection.updateOne(
  { <filter> },
  { $unset: { <field1>: "", <field2>: "", ... } }
)
```

- Removes specified fields from matched documents.

### 2. Positional `$` Operator — Update First Matching Array Element

```javascript
db.collection.updateOne(
  { <filter>, "arrayField.<field>": <value> },
  { $set: { "arrayField.$": <newValue> } }
)
```

- Updates the first array element matching the query condition.

### 3. Arithmetic Update Operators — Increment and Multiply

- Increment a numeric field:

```javascript
db.collection.updateOne(
  { <filter> },
  { $inc: { <field>: <amount> } }
)
```

- Multiply a numeric field:

```javascript
db.collection.updateOne(
  { <filter> },
  { $mul: { <field>: <multiplier> } }
)
```

### 4. findOneAndDelete — Atomic Find and Delete

```javascript
db.collection.findOneAndDelete(
  { <filter> },
  { projection: { <fields> }, sort: { <field>: 1 | -1 } }
)
```

- Finds one document matching filter, deletes it, and returns the deleted document atomically.

# Explanation

- **$unset:** Provides a way to remove unwanted or obsolete fields from documents without replacing the entire document.
- **Positional `$`:** This operator makes it possible to precisely update a specific element within an array matching filter criteria, avoiding wholesale array replacement.
- **Arithmetic operators:** `$inc` and `$mul` perform atomic, in-place updates on numeric fields ensuring data integrity and avoiding race conditions common in manual read-modify-write cycles.
- **findOneAndDelete:** Combines atomically finding and deleting a document into one operation, preventing race conditions and ensuring consistency, especially useful when deletion needs to return the deleted document for audit or further processing.

# Examples in mongosh and PyMongo

### $unset — Remove a Field

**mongosh:**

```javascript
db.users.updateOne({ name: "Alice" }, { $unset: { age: "" } })
```

**PyMongo:**

```python
from pymongo import MongoClient

client = MongoClient()
db = client.testdb

result = db.users.update_one({"name": "Alice"}, {"$unset": {"age": ""}})
print(f"Modified Count: {result.modified_count}")
```

***

### Positional `$` — Update First Matching Array Element

**mongosh:**

```javascript
db.posts.updateOne(
  { title: "MongoDB Tips", "comments.author": "Bob" },
  { $set: { "comments.$.approved": true } }
)
```

**PyMongo:**

```python
result = db.posts.update_one(
    {"title": "MongoDB Tips", "comments.author": "Bob"},
    {"$set": {"comments.$.approved": True}}
)
print(f"Modified Count: {result.modified_count}")
```

***

### $inc and $mul — Increment and Multiply Numeric Fields

**mongosh:**

```javascript
db.scores.updateOne({ user: "Carol" }, { $inc: { points: 10 } })
db.scores.updateOne({ user: "Carol" }, { $mul: { multiplier: 2 } })
```

**PyMongo:**

```python
db.scores.update_one({"user": "Carol"}, {"$inc": {"points": 10}})
db.scores.update_one({"user": "Carol"}, {"$mul": {"multiplier": 2}})
```

***

### findOneAndDelete — Atomic Delete and Return

**mongosh:**

```javascript
db.logs.findOneAndDelete({ level: "debug" })
```

**PyMongo:**

```python
deleted_doc = db.logs.find_one_and_delete({"level": "debug"})
print("Deleted Document:", deleted_doc)
```

# Best Practices While Using These Commands

- Use `$unset` cautiously to avoid accidental removal of important fields in documents.
- When using positional `$`, ensure the query filter correctly matches the intended array element to avoid updating wrong elements.
- Prefer atomic update operators like `$inc` and `$mul` over manual read-modify-write in application code to ensure concurrency safety.
- Use `findOneAndDelete` to avoid race conditions present when separately finding and deleting documents in multi-user environments.
- Always test updates in development environments before applying in production.

# Key Points for Exam and Interviews

- `$unset` removes specified document fields without affecting other data.
- The positional `$` operator updates the first array element matching filter criteria.
- `$inc` and `$mul` provide atomic, efficient numeric updates essential for counters and calculations.
- `findOneAndDelete` atomically deletes and returns a document, preventing concurrency issues.
- These commands cover critical MongoDB CRUD operations tested heavily on the certification exam and used frequently in real-world applications.

# Practice Questions (MCQs) — No Answers

1. What does the `$unset` operator do?  
a) Deletes the whole document  
b) Removes specified fields from documents  
c) Sets fields to null  

2. How does the positional `$` operator work in updates?  
a) Updates all elements of an array  
b) Updates the first matching array element  
c) Inserts a new array element  

3. Which operator increments a numeric field atomically?  
a) `$inc`  
b) `$set`  
c) `$mul`  

4. What does `findOneAndDelete` do?  
a) Finds and modifies a document  
b) Atomically finds, deletes, and returns a document  
c) Deletes multiple documents matching a filter  

5. Why prefer atomic update operators like `$inc`?  
a) For better query readability  
b) To avoid race conditions and ensure atomicity  
c) To update multiple documents  

# References

- [MongoDB $unset Operator](https://www.mongodb.com/docs/manual/reference/operator/update/unset/)  
- [MongoDB Positional $ Operator](https://www.mongodb.com/docs/manual/reference/operator/update/positional/)  
- [MongoDB $inc Operator](https://www.mongodb.com/docs/manual/reference/operator/update/inc/)  
- [MongoDB $mul Operator](https://www.mongodb.com/docs/manual/reference/operator/update/mul/)  
- [MongoDB findOneAndDelete](https://www.mongodb.com/docs/manual/reference/method/db.collection.findOneAndDelete/)