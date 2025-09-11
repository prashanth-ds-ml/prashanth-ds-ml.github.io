---
title : MongoDB Indexes and Explain Plan — Boosting Query Performance

---

# Introduction

Indexes are special data structures in MongoDB that improve the speed of query execution by allowing efficient location of data without scanning the entire collection. Understanding how to create, manage, and analyze indexes is crucial for optimizing database performance and forms a key portion of the MongoDB Associate Developer exam. The explain plan feature helps developers inspect query execution and understand whether indexes are used effectively or if collection scans are occurring.[1]

***

# Indexes Basics and Types

- **Single Field Index:** Created on one field; speeds up exact match or range queries on that field.
- **Compound Index:** Includes multiple fields, useful when queries filter on multiple keys.
- **Multikey Index:** Automatically created for fields that are arrays, indexing each array element.
- **Hashed Index:** Supports sharding by hashing values of a field.
- **Text Index:** Specialized for search on string content.

Indexes reduce data examined and accelerate query responses but come with write overhead and storage cost.[1]

***

# Creating and Managing Indexes

- Create an index using:

```js
db.collection.createIndex({ field: 1 })  // 1 for ascending order
```

- List indexes:

```js
db.collection.getIndexes()
```

- Drop index:

```js
db.collection.dropIndex({ field: 1 })
```

Efficient index design considers query patterns, selectivity, and index cardinality.[1]

***

# Explain Plan — Analyzing Query Execution

- Use `.explain()` to obtain the query execution plan, detailing if a query used an index or performed a collection scan.

Example:

```js
db.collection.find({ field: value }).explain("executionStats")
```

- Key explain output fields:
  - **stage:** The type of operation (e.g., IXSCAN for index scan, COLLSCAN for collection scan).
  - **indexName:** The index used (if any).
  - **totalKeysExamined:** Number of index entries scanned.
  - **totalDocsExamined:** Number of documents scanned.

- Presence of COLLSCAN implies poor index usage and potential performance bottlenecks and requires adding or optimizing indexes.[1]

***

# Best Practices and Considerations

- Always analyze explain output for slow or large queries.
- Avoid over-indexing as it impacts write performance.
- Use compound indexes wisely for multi-criteria queries.
- Consider index coverage to fulfill queries entirely from indexes without fetching documents.
- Keep indexes aligned with query patterns and sort operations.

***

# Practice Examples

- Create a compound index on "year" (ascending) and "rating" (descending):

```js
db.movies.createIndex({ year: 1, rating: -1 })
```

- Check indexes on movies collection:

```js
db.movies.getIndexes()
```

- Explain a query filtering on year and rating:

```js
db.movies.find({ year: { $gte: 2000 }, rating: { $gte: 8 } }).explain("executionStats")
```

***

# Practice Questions

1. What does a COLLSCAN stage in the explain plan indicate?  
a) Query uses an index scan  
b) Query performs a full collection scan  
c) Query uses an aggregation pipeline  

2. Which command lists all indexes on a collection?  
a) db.collection.listIndexes()  
b) db.collection.getIndexes()  
c) db.collection.showIndexes()  

3. What is a compound index?  
a) An index on a single field  
b) An index spanning multiple fields  
c) An index on array elements  

4. What is a potential downside of having too many indexes?  
a) Increased read speed  
b) Increased storage and slower writes  
c) Improved write speed  

***

# References

- [MongoDB Official Docs: Indexes](https://www.mongodb.com/docs/manual/indexes/)[1]
- [MongoDB Official Docs: Explain Results](https://www.mongodb.com/docs/manual/reference/explain-results/)[2]
- [MongoDB Official Docs: Compound Indexes](https://www.mongodb.com/docs/manual/core/index-compound/)[3]

