---
title: Mastering MongoDB Logical Operators — $and, $or, $not, $nor
---

# Mastering MongoDB Logical Operators — $and, $or, $not, $nor

Welcome back to the MongoDB learning series!  
Today, we dive into the powerful **logical operators** in MongoDB: `$and`, `$or`, `$not`, and `$nor`. These operators allow you to compose complex queries by combining multiple conditions — an essential skill for the MongoDB Associate Developer exam and effective querying.

***

## 1. Explanation

Logical operators help specify how multiple criteria interact to match documents:

- **`$and`**: Matches documents where *all* conditions are true.
- **`$or`**: Matches documents where *any* condition is true.
- **`$not`**: Negates a given condition.
- **`$nor`**: Matches documents where *none* of the conditions are true.

These operators enable flexible, precise filtering beyond simple conditions.

***

## 2. Syntax Breakdown and Explanation

| Operator  | Syntax Example                                        | Description                                                        |
|-----------|-----------------------------------------------------|------------------------------------------------------------------|
| **`$and`**  | `{ $and: [ {cond1}, {cond2}, ... ] }`                 | Joins multiple conditions by AND; docs must satisfy *all*       |
| **`$or`**   | `{ $or: [ {cond1}, {cond2}, ... ] }`                  | Joins conditions by OR; docs must satisfy *any*                  |
| **`$not`**  | `{ field: { $not: { <condition> } } }`                | Negates condition on a field; matches docs where condition is false |
| **`$nor`**  | `{ $nor: [ {cond1}, {cond2}, ... ] }`                  | Joins conditions by NOR; docs satisfy *none* of the conditions   |

***

## 3. Real-Life Examples

### Example 1: Find movies released after 2010 *and* rated above 8

**Mongo Shell:**

```js
db.movies.find({
  $and: [
    { year: { $gt: 2010 } },
    { "imdb.rating": { $gt: 8 } }
  ]
})
```

**PyMongo:**

```python
from pymongo import MongoClient

client = MongoClient('mongodb://localhost:27017/')
db = client.sample_mflix

cursor = db.movies.find({
    "$and": [
        {"year": {"$gt": 2010}},
        {"imdb.rating": {"$gt": 8}}
    ]
})

for movie in cursor:
    print(movie["title"], movie["year"], movie["imdb"]["rating"])
```

***

### Example 2: Find movies released after 2010 *or* belonging to Sci-Fi genre

**Mongo Shell:**

```js
db.movies.find({
  $or: [
    { year: { $gt: 2010 } },
    { genres: "Sci-Fi" }
  ]
})
```

**PyMongo:**

```python
cursor = db.movies.find({
    "$or": [
        {"year": {"$gt": 2010}},
        {"genres": "Sci-Fi"}
    ]
})

for movie in cursor:
    print(movie["title"], movie.get("genres"))
```

***

### Example 3: Find movies NOT rated below PG-13

**Mongo Shell:**

```js
db.movies.find({ rated: { $not: { $lt: "PG-13" } } })
```

**PyMongo:**

```python
cursor = db.movies.find({
    "rated": {"$not": {"$lt": "PG-13"}}
})

for movie in cursor:
    print(movie["title"], movie.get("rated"))
```

***

### Example 4: Find movies neither comedies nor dramas

**Mongo Shell:**

```js
db.movies.find({
  $nor: [
    { genres: "Comedy" },
    { genres: "Drama" }
  ]
})
```

**PyMongo:**

```python
cursor = db.movies.find({
    "$nor": [
        {"genres": "Comedy"},
        {"genres": "Drama"}
    ]
})

for movie in cursor:
    print(movie["title"], movie.get("genres"))
```

***

## 4. Key Points for Exam & Interviews

- MongoDB applies implicit AND between fields; explicit `$and` is used mostly for complex or nested conditions.
- `$or` and `$nor` arrays require conditions to define alternatives or exclusions.
- `$not` negates conditions on a single field and cannot replace `$nor` or `$and`.
- Logical operators support nesting and combination to build complex queries.
- Understanding operator precedence and logic improves query accuracy.
- Logical operators often appear as query construction or output recognition questions on exams.

***

## 5. Practice MCQs (No answers provided)

**Q1:** Which operator matches documents where *at least one* listed condition is true?  
a) `$and`  
b) `$or`  
c) `$not`  
d) `$nor`  

***

**Q2:** What does the following query return?  
```js
db.movies.find({
  $nor: [{ genres: "Comedy" }, { genres: "Horror" }]
})
```
a) Movies that are Comedy or Horror  
b) Movies that are neither Comedy nor Horror  
c) Only movies labeled Horror  
d) No documents at all  

***

**Q3:** How is the `$not` operator typically used?  
a) To negate a field-specific condition  
b) To combine two or more conditions with OR  
c) To reverse the effect of the entire query  
d) To match documents where field exists  

***

## 6. References

- [MongoDB $and Operator](https://www.mongodb.com/docs/manual/reference/operator/query/and/)  
- [MongoDB $or Operator](https://www.mongodb.com/docs/manual/reference/operator/query/or/)  
- [MongoDB $not Operator](https://www.mongodb.com/docs/manual/reference/operator/query/not/)  
- [MongoDB $nor Operator](https://www.mongodb.com/docs/manual/reference/operator/query/nor/)  

***

Let me know if you want to proceed with the next blog or anything else!