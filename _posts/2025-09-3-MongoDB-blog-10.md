---
title: MongoDB CRUD — Updating Multiple Documents
---

***

# MongoDB CRUD —  Updating Multiple Documents

Welcome back to the MongoDB learning series! In this post, we dive into **updating multiple documents simultaneously**, a common requirement in real-world applications and an important topic for the MongoDB Associate Developer Exam.

***

## Scenario

Imagine you want to update a field across many documents that match a criteria — for example, adding a new tag to all “Action” genre movies or increasing the stock count of all products from a specific supplier.

MongoDB provides the `updateMany` command for performing bulk updates safely and efficiently.

***

## Syntax

```js
db.<collection>.updateMany(
  <filter>,
  <update>,
  <options>
)
```

- `<filter>`: Query to select documents to update.
- `<update>`: Update operators (e.g., `$set`, `$inc`).
- `<options>`: Optional settings (e.g., `upsert`).

***

## Hands-On Example: Add “Classic” Tag to All Movies Before 2000

Using the sample dataset `sample_mflix.movies`:

```js
db.movies.updateMany(
  { year: { $lt: 2000 } },
  { $addToSet: { tags: "Classic" } }
)
```

### Explanation:

- Filter selects movies with `year` less than 2000.
- `$addToSet` adds `"Classic"` to the `tags` array if not already present.
- All matching documents are updated.

***

## Result Verification

Find a movie before 2000 to check:

```js
db.movies.find({ year: { $lt: 2000 } }).limit(3).pretty()
```

You'll see the `"Classic"` tag added to their `tags` array.

***

## PyMongo Equivalent

```python
from pymongo import MongoClient

client = MongoClient("mongodb://localhost:27017/")
db = client.sample_mflix

db.movies.update_many(
    {"year": {"$lt": 2000}},
    {"$addToSet": {"tags": "Classic"}}
)
```

***

## Important Notes

- `updateMany` updates **all** documents matching the filter.
- Use update operators like `$set`, `$inc`, `$addToSet` to modify fields appropriately.
- Avoid omitting update operators; a plain replacement document would overwrite documents.
- Bulk updates can be performance-intensive; indexing your filter fields helps.

***

## Key Points for the Exam and Interviews

- `updateMany` allows modification of multiple documents matching the filter.
- Update operators ensure partial, targeted updates without overwriting entire documents.
- Bulk updates must be used carefully to prevent unintended data changes.
- Commonly tested in exams: identifying the right update command to apply and its effect.
- Understanding update operators alongside bulk updates is critical.

***

## Practice Questions (No Answers)

1. Which command updates multiple documents matching a filter?

a) updateOne

b) updateMany

c) insertMany

2. What happens if you run updateMany without an update operator (like `$set`)?

a) Partial update is performed

b) Matched documents are replaced with the provided document

c) Error is thrown

3. Given the command:

```js
db.products.updateMany(
  { category: "Electronics" },
  { $inc: { stock: 10 } }
)
```

What is the effect?

a) All Electronics products have their stock increased by 10

b) Only one Electronics product is updated

c) All products have stock reset to 10

***

## References

- MongoDB Docs: [updateMany](https://www.mongodb.com/docs/manual/reference/method/db.collection.updateMany/)  
- MongoDB Docs: [Update Operators](https://www.mongodb.com/docs/manual/reference/operator/update/)  
- Previous Blogs: [Blog 8: `$set` Operator](#) | [Blog 9: Upsert](#)  

***
