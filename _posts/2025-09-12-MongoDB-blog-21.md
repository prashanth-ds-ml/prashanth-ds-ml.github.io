---
title: Counting Documents in MongoDB — Expressions and Usage
---

# Counting Documents Matching a Query in MongoDB

This blog covers how to count documents that match specific query criteria in MongoDB, a key skill for the MongoDB Associate Developer exam (topic 2.17). While querying documents is commonly discussed, understanding the correct expressions to count matching documents and their distinctions is equally essential for both exam and practical usage.

***

## Common Expressions to Count Documents

### 1. `count()` Method (Deprecated)

- Syntax:

```javascript
db.collection.count(query)
```

- Counts documents matching the `query`.
- Deprecated in latest MongoDB versions; may not reflect accurate results in sharded clusters.
- Example:

```javascript
db.movies.count({ year: { $gte: 2010 } })
```

Counts movies released in or after 2010.

***

### 2. `countDocuments()` Method (Recommended)

- Syntax:

```javascript
db.collection.countDocuments(query)
```

- Returns an accurate count of documents matching the filter.
- Uses the query execution system, reflecting current state accurately.
- Example:

```javascript
db.movies.countDocuments({ genres: "Action" })
```

Counts movies that include `"Action"` genre.

***

### 3. `estimatedDocumentCount()` Method

- Syntax:

```javascript
db.collection.estimatedDocumentCount()
```

- Provides a fast estimate of the total documents in the collection.
- Does not take a query parameter.
- Useful for rough counts.
- Example:

```javascript
db.movies.estimatedDocumentCount()
```

Returns an approximate total count of movies.

***

### 4. Aggregation Pipeline for Counting

- Using aggregation provides flexibility to count with grouping, filtering, and complex conditions.

- Syntax:

```javascript
db.collection.aggregate([
  { $match: <query> },
  { $count: "total" }
])
```

- Example:

```javascript
db.movies.aggregate([
  { $match: { year: { $gte: 2010 } }},
  { $count: "count" }
])
```

Returns a document `{ count: <number> }` representing matched document count.

***

## Practical Examples

### Using `countDocuments()`

```javascript
const actionMoviesCount = db.movies.countDocuments({ genres: "Action" });
print("Number of Action movies:", actionMoviesCount);
```

### Using Aggregation for Count

```javascript
const result = db.movies.aggregate([
  { $match: { "imdb.rating": { $gte: 8 } } },
  { $count: "highRatedMovies" }
]).toArray();

printjson(result);
```

***

## Key Differences and Recommendations

| Expression             | Accepts Query | Accurate for Sharded Collections | Includes Filter | Deprecation Status  |
|------------------------|---------------|---------------------------------|-----------------|--------------------|
| `count()`              | Yes           | No                              | Yes             | Deprecated         |
| `countDocuments()`     | Yes           | Yes                             | Yes             | Recommended        |
| `estimatedDocumentCount()` | No         | Yes                             | No              | Recommended for totals |

***

## PyMongo Examples

```python
from pymongo import MongoClient

client = MongoClient('mongodb://localhost:27017/')
db = client.sample_mflix

# Count documents matching genre "Drama"
count_drama = db.movies.count_documents({"genres": "Drama"})
print("Drama movies count:", count_drama)

# Aggregation count
pipeline = [
    {"$match": {"year": {"$gte": 2015}}},
    {"$count": "countRecentMovies"}
]

result = list(db.movies.aggregate(pipeline))
if result:
    print("Recent movies count:", result[0]["countRecentMovies"])
else:
    print("No matching documents found.")
```

***

## Key Points for Exam and Interviews

- `countDocuments()` is the preferred method to count matching documents accurately.
- `count()` is deprecated and less reliable in modern MongoDB deployments.
- Aggregation with `$count` stage offers flexibility to combine filtering and counting.
- Understand when to use estimated counts (`estimatedDocumentCount()`) versus filtered counts.
- Accurate counting is crucial for analytics, pagination, and reporting.

***

## Practice Questions (No Answers)

1. Which MongoDB method accurately counts documents matching a filter in sharded collections?

a) `count()`  
b) `countDocuments()`  
c) `estimatedDocumentCount()`  

2. What does the `$count` stage do in an aggregation pipeline?

a) Filters documents  
b) Returns the total number of input documents at that stage  
c) Sorts documents  

3. Which method provides a fast estimate of total documents in a collection without filters?

a) `count()`  
b) `countDocuments()`  
c) `estimatedDocumentCount()`  

4. How would you count documents with `year` greater than 2020 using aggregation?

a) Use `$group` with `$sum`  
b) Use `$match` then `$count`  
c) Use `countDocuments()` only  

***
