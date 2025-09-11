---

title: MongoDB Indexes — Compound Indexes and Explain Plan Deep Dive

---

## Introduction

Indexes are essential for speeding up queries in MongoDB. While single-field indexes optimize queries on one field, **compound indexes** cover multiple fields and enable efficient execution of queries filtering or sorting on combinations of fields. Knowledge of compound index design and interpreting explain plans is crucial for MongoDB certification and real-world data performance tuning.[1]

***

## Compound Indexes

A compound index is an index on multiple fields within a document.

### Syntax to Create a Compound Index

```js
db.collection.createIndex({ field1: 1, field2: -1 })
```

- `field1: 1` indicates ascending order.
- `field2: -1` indicates descending order.
- Order matters: queries use the index efficiently only if they filter or sort on the index prefix fields in the defined order.

### Usage Examples

- Index Definition:

```js
db.movies.createIndex({ year: 1, rating: -1 })
```

- Query that benefits:

```js
db.movies.find({ year: 2020, rating: { $gte: 8 } })
```

***

## Explain Plan Syntax and Usage

Use `.explain()` method to analyze query execution:

```js
db.collection.find(<query>).explain("executionStats")
```

- `"executionStats"` mode provides detailed stats.

### Explain Plan Key Fields

- `stage`: Indicates execution stage (e.g., `IXSCAN` for index scan, `COLLSCAN` for collection scan).
- `indexName`: Name of the index used.
- `keyPattern`: The fields and sort order in the index.
- `totalKeysExamined`: Number of index entries scanned.
- `totalDocsExamined`: Number of documents examined.
- `executionTimeMillis`: Time taken for the query to execute.

***

## Best Practices for Compound Indexes

- Put equality conditions first in the index definition, followed by range conditions.
- Use compound indexes to support queries that filter/sort on multiple fields.
- Avoid unnecessary indexes to reduce write overhead.
- Use explain plans regularly to verify index usage and detect collection scans.
- Aim for covered queries where all requested fields are in the index.

***

## Key Points for Exam and Interviews

- Compound indexes speed up queries that filter on multiple fields.
- Index field order affects query compatibility and index efficiency.
- Explain plans reveal whether queries use indexes or perform collection scans.
- Efficient indexing improves read performance but adds write cost.
- Covered queries serve from index alone, improving performance.
- Understanding these concepts is often tested in certification exams and interviews.

***

## Practice Questions

1. What does the order of fields in a compound index affect?  
a) Query compatibility and index efficiency  
b) Index size only  
c) MongoDB sorts them internally  

2. Which explain plan stage means an index scan was performed?  
a) COLLSCAN  
b) IXSCAN  
c) FETCH  

3. Can a compound index on `{ year: 1, rating: -1 }` support queries filtering only by `rating`?  
a) Yes  
b) No  

4. What is a covered query?  
a) Query reads only the index data without fetching documents  
b) Query fetches documents after index scan  
c) MongoDB performs a collection scan  

***

## References

- [MongoDB Official Docs — Compound Indexes](https://www.mongodb.com/docs/manual/core/index-compound/)[1]
- [MongoDB Official Docs — Explain Results](https://www.mongodb.com/docs/manual/reference/explain-results/)[2]
- [MongoDB Official Docs — Indexes](https://www.mongodb.com/docs/manual/indexes/)[3]

***
