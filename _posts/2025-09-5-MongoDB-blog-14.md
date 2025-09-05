---
title: MongoDB Array Query Operators — $in and $elemMatch
---

# MongoDB Array Query Operators — $in and $elemMatch

Welcome back to the MongoDB learning series!  
Today, we explore powerful array query operators `$in` and `$elemMatch`. These are essential for querying documents with arrays and multi-criteria matches, an important skill for the MongoDB Associate Developer exam and real-world tasks.

***

## 1. Explanation

Arrays are fundamental BSON types in MongoDB. Querying array fields requires special operators beyond simple equality.

- The `$in` operator matches documents where a field’s value equals any in a specified list. It's useful for checking if any values from a list are present in a field.
- The `$elemMatch` operator is used to find documents where at least one element in an array satisfies multiple conditions, often involving compound criteria.

***

## 2. Syntax Breakdown

### The `$in` Operator

```js
{ field: { $in: [value1, value2, ...] } }
```

- Matches documents where `field` is equal to *any* value in the provided array.
- Works with scalar fields or fields containing arrays.

### The `$elemMatch` Operator

```js
{ arrayField: { $elemMatch: { condition1, condition2, ... } } }
```

- Matches documents where *at least one* element of `arrayField` satisfies *all* specified conditions.
- Used especially for querying array of objects or matching on multiple criteria in one array element.

***

## 3. Real-Life Examples

### Example 1: Find Movies with Genres in ["Drama", "Comedy"] (Mongo Shell)

```js
db.movies.find({ genres: { $in: ["Drama", "Comedy"] } })
```

This finds movies where the `genres` array contains *at least one* of `"Drama"` or `"Comedy"`.

### Example 2: Find Movies with an "Action" Genre and IMDb Rating Over 8 (Mongo Shell)

```js
db.movies.find({
  genres: { $elemMatch: { $eq: "Action" } },
  "imdb.rating": { $gt: 8 }
})
```

This finds movies having `"Action"` in their genres and an IMDb rating greater than 8.  

**Note:**  
Here `$elemMatch` enables matching array elements cleanly. If querying embedded documents arrays, you can specify multiple conditions inside `$elemMatch`.

### Example 3: PyMongo Code to Find Movies with Comedy or Drama Genres

```python
from pymongo import MongoClient

client = MongoClient('mongodb://localhost:27017/')
db = client.sample_mflix

cursor = db.movies.find({ "genres": { "$in": ["Drama", "Comedy"] } })
for movie in cursor:
    print(movie["title"], movie.get("genres"))
```

***

## 4. Key Points for Exam and Interviews

- `$in` behaves like an OR for specified values, easy for simple array inclusion.
- `$elemMatch` allows specifying multiple conditions for *the same* array element.
- `$elemMatch` is necessary when matching complex array of subdocuments or conditional queries.
- Understand the type of array contents (primitive values vs objects) for correct operator choice.
- Logical operators can be combined with `$in` and `$elemMatch` for layered queries.

***

## 5. Practice MCQs

1. What does `$in` operator in query do?

a) Matches documents where field contains *all* values in array  

b) Matches documents where field matches *any* value in array  

c) Matches documents where field is *equal* to the first array element  

d) Matches documents with arrays larger than specified array

***

2. How does `$elemMatch` differ from `$in` when querying arrays?

a) `$elemMatch` matches multiple conditions in a *single* element  

b) `$elemMatch` matches any element in an array with listed values  

c) `$elemMatch` replaces `$in` functionality  

d) `$elemMatch` only works with numeric arrays

***

3. Given the query:

```js
db.movies.find({ genres: { $elemMatch: { $eq: "Action" } } })
```

What movies will match?

a) Movies with `"Action"` as one genre in `genres` array  

b) Movies with `"Action"` as *all* genres  

c) Movies where all genres match exactly `"Action"`  

d) No movies, query is invalid

***

## 6. References

- [MongoDB Docs: $in operator](https://www.mongodb.com/docs/manual/reference/operator/query/in/)  
- [MongoDB Docs: $elemMatch operator](https://www.mongodb.com/docs/manual/reference/operator/query/elemMatch/)

***

