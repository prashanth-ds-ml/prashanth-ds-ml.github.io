---
title: MongoDB Query Operators — Equality and Relational Expressions
---

# MongoDB Query Operators — Equality and Relational Expressions

Welcome back to the MongoDB learning series!  
This post focuses on mastering some of the most fundamental query operators in MongoDB — *equality* and *relational* operators. These operators form the backbone of effectively filtering documents and are essential for day-to-day querying and the MongoDB Associate Developer exam.

***

## 1. Explanation

MongoDB allows you to query documents by matching fields with specific values or by applying comparative conditions. The simplest way is using *equality*, where you specify a field and the exact value it should have. More advanced filters use *relational operators* like greater than, less than, greater than or equal to, and less than or equal to for numeric or date fields.

***

## 2. Syntax Breakdown and Explanation

In MongoDB, query operators are specified as key-value pairs within your query document. You build queries by combining field names with operators that describe how the matching should be performed.

### Basic Equality Query

```js
{ field: value }
```

- Matches documents where `field` equals the specified `value`.
- Example: `{ year: 2010 }` finds documents with `year` exactly equal to 2010.
- Note: MongoDB queries are *type-sensitive*, so the type of `value` must match the field’s stored type, or no match will occur.

### Common Comparison Operators

| Operator | Description                                     | Example                     |
|----------|-------------------------------------------------|-----------------------------|
| `$eq`    | Matches values exactly equal to a specified value | `{ year: { $eq: 2010 } }`   |
| `$ne`    | Matches values not equal to a specified value     | `{ year: { $ne: 2010 } }`   |
| `$gt`    | Matches values greater than a specified value     | `{ year: { $gt: 2015 } }`   |
| `$gte`   | Matches values greater than or equal to a value   | `{ year: { $gte: 2010 } }`  |
| `$lt`    | Matches values less than a specified value        | `{ year: { $lt: 2000 } }`   |
| `$lte`   | Matches values less than or equal to a value      | `{ year: { $lte: 2005 } }`  |

- Operators are *inclusive* or *exclusive* depending on the operator.
- Multiple comparison operators can be combined on a single field; for example:

```js
{ year: { $gte: 2010, $lt: 2020 } }
```

This query matches documents where `year` is between 2010 (inclusive) and 2020 (exclusive).

### Correct Operator Usage

Comparison operators must be tied to a field name:

Incorrect:

```js
{ $gt: 2010 } // Not tied to a field, invalid syntax
```

Correct:

```js
{ year: { $gt: 2010 } }
```

### Type Matching

- MongoDB requires the query value type to match the stored field type.
- Querying `{ year: "2010" }` when `year` is stored as a number results in *no matches*.
- Numeric comparisons work with numbers and dates. String comparisons are lexicographical.

### Combining Criteria

Multiple conditions can be combined within the same query document:

```js
{ year: { $gte: 2010, $lte: 2020 }, "imdb.rating": { $gt: 7 } }
```

Finds movies released between 2010 and 2020 inclusive with IMDb score above 7.

***

## 3. Real-Life Examples

### Example 1: Find Movies Released After 2015 (Mongo Shell)

```js
db.movies.find({ year: { $gt: 2015 } })
```

### Example 2: Find Movies Released Between 2000 and 2010 (Mongo Shell)

```js
db.movies.find({ year: { $gte: 2000, $lte: 2010 } })
```

### Example 3: Find Movies with IMDb Rating at Least 8.5 (Mongo Shell)

```js
db.movies.find({ "imdb.rating": { $gte: 8.5 } })
```

### Example 4: PyMongo Query — Find Movies Released After 2015 (Python)

```python
from pymongo import MongoClient

client = MongoClient('mongodb://localhost:27017/')
db = client.sample_mflix

cursor = db.movies.find({ "year": { "$gt": 2015 } })
for movie in cursor:
    print(movie["title"], movie["year"])
```

***

## 4. Key Points for Exam and Interviews

- MongoDB queries are **type sensitive**; mismatched types yield no results.
- Use relational operators `$gt`, `$gte`, `$lt`, and `$lte` for numeric and date ranges.
- Combine multiple comparison operators for accurate range queries.
- Operator syntax requires the operator nested inside the field’s query object.
- Proper use of comparison operators optimizes query effectiveness and index use.
- Understanding MongoDB's BSON types is crucial for correct and performant queries.

***

## 5. Multiple Choice Questions (No Answers)

**Q1:** Which MongoDB query matches documents where `year` is exactly 2010?  
a) `{ year: "2010" }`  
b) `{ year: 2010 }`  
c) `{ year: { $eq: "2010" } }`  
d) `{ year: { $neq: 2010 } }`

**Q2:** What does the query `{ year: { $gte: 2000, $lt: 2010 } }` return?  
a) Movies released before 2000 or after 2010  
b) Movies released from 2000 inclusive to 2010 exclusive  
c) Movies released exactly in 2000 and 2010  
d) Movies released only before 2010

**Q3:** Which statement about MongoDB queries is true?  
a) Queries ignore data type differences  
b) Queries require exact type matches  
c) Numeric queries accept string input automatically  
d) Queries do not support comparison operators

***

## 6. References

- [MongoDB Official Docs: Query and Projection Operators](https://www.mongodb.com/docs/manual/reference/operator/query/)  
- [MongoDB Official Docs: Comparison Query Operators](https://www.mongodb.com/docs/manual/reference/operator/query-comparison/)  

***
