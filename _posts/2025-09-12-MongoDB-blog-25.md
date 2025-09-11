---
title: Identifying and Interpreting Indexes in a MongoDB Collection
---

# How to Identify the Number of Indexes for a Collection and Understand Their Implications

Indexes are fundamental to MongoDB’s query performance, and understanding how many and which indexes exist on a collection is essential for database optimization and maintenance. This blog focuses on **identifying the number of indexes on a collection**, aligned with exam topic 3.4, and provides insights into interpreting index counts and their operational impact.

***

## Listing Indexes with `getIndexes()`

- The primary MongoDB shell command to list all indexes created on a collection is:

```js
db.collection.getIndexes()
```

- This command returns an array of index specification documents, each describing an index on the collection.

***

## Example: Listing Indexes for the `movies` Collection

```js
db.movies.getIndexes()
```

Sample output:

```json
[
  {
    "v": 2,
    "key": { "_id": 1 },
    "name": "_id_",
    "ns": "sample_mflix.movies"
  },
  {
    "v": 2,
    "key": { "title": 1 },
    "name": "title_1",
    "ns": "sample_mflix.movies"
  },
  {
    "v": 2,
    "key": { "year": -1, "rating": 1 },
    "name": "year_-1_rating_1",
    "ns": "sample_mflix.movies"
  }
]
```

- The response shows three indexes: the default `_id` index plus two additional indexes on `title` and compound keys `year` and `rating`.

***

## Counting Indexes

- To **count the number of indexes**, you can simply inspect the length of the returned array:

```js
db.movies.getIndexes().length
```

- This returns an integer number of indexes defined on the collection.

***

## PyMongo Equivalent

```python
from pymongo import MongoClient

client = MongoClient("mongodb://localhost:27017/")
db = client.sample_mflix

indexes = db.movies.list_indexes()
index_list = list(indexes)
print(f"Number of indexes: {len(index_list)}")

for idx in index_list:
    print(idx)
```

***

## Implications of Index Counts

### 1. There is Always at Least One Index: `_id`

- Every MongoDB collection has a built-in unique index on the `_id` field.
- This index is mandatory for document uniqueness and lookup efficiency.

### 2. Large Numbers of Indexes Can Impact Performance

- More indexes mean additional disk space and slower writes due to index maintenance.
- Excessive or unused indexes can degrade overall database efficiency.

### 3. Understanding Index Purpose by Analyzing Naming and Key Patterns

- Index `name` often indicates fields indexed and order (e.g., `year_-1_rating_1`).
- Index `key` shows fields and directions (1 for ascending, -1 for descending).
- This helps determine which queries benefit from which indexes.

### 4. Index Count Reflects Query Optimization Efforts

- Fewer, well-designed indexes typically outperform many overlapping or redundant indexes.
- Balancing index count with workload type (read-heavy vs write-heavy) is essential.

***

## Best Practices for Index Identification and Management

- Regularly run `getIndexes()` or `listIndexes()` to audit active indexes.
- Use tools like MongoDB Atlas Performance Advisor to detect unused or redundant indexes.
- Remove unnecessary indexes to improve write performance and reduce storage.
- Monitor index build times for large collections.
- Document index purposes clearly if maintaining many.

***

## Summary Table of Common Index Count Scenarios

| Scenario                        | Typical Index Count Description                     | Performance Impact                            |
|--------------------------------|---------------------------------------------------|----------------------------------------------|
| New collection                 | Single mandatory `_id` index                       | Minimal overhead                              |
| Read-heavy application         | Multiple indexes targeting frequent query patterns | Improved reads, higher writes overhead       |
| Write-heavy application        | Minimal indexes beyond `_id`                        | Higher write throughput, slower complex queries |
| Poor index hygiene             | Many unused or overlapping indexes                  | High storage, slow writes, complex plans     |

***

## Key Exam & Interview Points

- `getIndexes()` and `listIndexes()` commands reveal all indexes for a collection.
- Index count always ≥ 1 due to default `_id` index.
- Understanding number and type of indexes aids in diagnosing performance issues.
- Fewer, targeted indexes help balance query speed and write cost.
- Counting and reviewing indexes should be part of routine maintenance and tuning.

***

## Practice Questions (No Answers)

1. What command lists all indexes on a MongoDB collection?  
a) `db.collection.listIndexes()`  
b) `db.collection.getIndexes()`  
c) Both a and b  
d) `db.collection.indexes()`

2. What is the minimum number of indexes every MongoDB collection has?  
a) Zero  
b) One (_id index)  
c) It varies by collection  
d) Two

3. Why can having too many indexes be harmful?  
a) It reduces read performance  
b) It increases write latency and storage requirements  
c) It causes query failures  

4. How can you find out the number of indexes defined on a collection using the shell?  
a) Count elements in the array returned by `getIndexes()`  
b) Use `db.collection.count()`  
c) Use `db.collection.stats()` only  

5. What information can you get from an index definition document?  
a) Indexed fields and their sort order  
b) Index name  
c) Namespace linked to the collection  
d) All of the above  

***

