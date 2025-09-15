---
title: MongoDB Advanced Cursor Operations
---
# Introduction

This blog continues the series on advanced CRUD and cursor handling in MongoDB, a critical skill area for the MongoDB Associate Developer exam. Here, focus shifts to mastering cursor mechanics with `sort()`, `skip()`, `limit()`, and `batchSize()`, as well as best practices for iterating and closing cursors. These techniques are essential for efficient data retrieval, pagination, and building scalable applications.

# Syntax Breakdown

### 1. Sorting Cursor Results

```javascript
db.collection.find(<query>).sort({ <field1>: 1, <field2>: -1 })
```
- `1` for ascending, `-1` for descending order.

### 2. Limiting Results

```javascript
db.collection.find(<query>).limit(<number>)
```
- Restricts the number of documents returned.

### 3. Skipping Results

```javascript
db.collection.find(<query>).skip(<number>)
```
- Skips a specified number of documents, ideal for pagination.

### 4. Batch Size

```javascript
db.collection.find(<query>).batchSize(<size>)
```
- Controls documents returned per batch between server and client.

### 5. Cursor Iteration & Closing

- Default: MongoDB drivers auto-iterate cursors.
- Manual: Use `.next()` or Python's `for doc in cursor:`.
- Close: `cursor.close()`, or automatically after reading all results or after timeout.

# Explanation of Usage

- **sort():** Orders results as per specified fields, critical for returning ranked or paginated data.
- **skip():** Useful in conjunction with `limit()` and `sort()` for paginated UI; however, high skip values can impact performance.
- **limit():** Prevents transferring or loading excessive data at once, improving speed and resource use.
- **batchSize():** Adjusts the number of documents fetched per client-server roundtrip for fine-tuned performance, especially in large datasets or streaming pipelines.
- **Cursor Iteration:** Efficiently processes large result sets without loading all documents into memory. Always close unused cursors early to conserve server resources.

# Practical Examples (mongosh and PyMongo)

**Mongosh: Get Page 2, 5 results, sorted by name**

```javascript
db.users.find().sort({ name: 1 }).skip(5).limit(5)
```

**PyMongo: Paginated Query and Iteration**

```python
from pymongo import MongoClient

client = MongoClient()
db = client.testdb

# Get 2nd page (documents 6-10) sorted by 'name'
cursor = db.users.find({}).sort("name", 1).skip(5).limit(5)

# Iterate and print
for doc in cursor:
    print(doc)
cursor.close()  # Explicit close
```

***

**Mongosh: Setting batch size**

```javascript
db.orders.find({ status: "pending" }).batchSize(100)
```

**PyMongo: Batch Processing**

```python
cursor = db.orders.find({"status": "pending"}).batch_size(100)
for order in cursor:
    process(order)
```

***

**Efficient Large Query with Pagination**

*Retrieve third page of books (10 per page), sorted descending by publish date:*

**Mongosh:**
```javascript
db.books.find().sort({ publish_date: -1 }).skip(20).limit(10)
```

**PyMongo:**
```python
books = db.books.find().sort("publish_date", -1).skip(20).limit(10)
for book in books:
    print(book["title"])
```

# Best Practices

- Always use `sort()` with pagination for consistent ordering.
- Combine `skip()` and `limit()` for reliable pagination but monitor performance on large offsets.
- Adjust `batchSize()` for bulk processing or streaming to reduce latency and resource load.
- Explicitly close long-lived cursors to free server resources if not fully iterated.
- For extremely large page numbers or datasets, consider range-based pagination for efficiency.

# Key Points for Exam and Interviews

- `sort()`, `limit()`, and `skip()` are fundamental for controlled, paginated, and ordered results in queries.
- `batchSize()` optimizes data transfer between client and server for large result sets.
- Proper cursor iteration (manual or automatic) avoids memory issues with big collections.
- Closing unused cursors promptly avoids cursor leaks.
- These cursor features are standard for efficient data pipelines, reporting UIs, and RESTful services on MongoDB.

# Practice Questions (MCQs) — No Answers

1. What does `limit()` do in a MongoDB query?  
a) Limits the fields returned  
b) Limits the number of documents returned  
c) Skips the first n documents

2. How does `skip()` help in pagination?  
a) It limits returned results  
b) It skips a set number of documents starting from the sort order  
c) It sorts results alphabetically

3. What is the advantage of tuning `batchSize()` in queries?  
a) Controls the server RAM usage  
b) Alters the number of documents returned per network round trip  
c) Limits concurrent clients

4. Why should you always include `sort()` when paginating results?  
a) To ensure consistent and repeatable result order across requests  
b) It is required for batchSize to work  
c) It changes document IDs

5. How do you close a PyMongo cursor explicitly?  
a) `cursor.delete()`  
b) `cursor.close()`  
c) `del cursor`

# References

- [MongoDB Cursor Manual](https://www.mongodb.com/docs/manual/reference/method/js-cursor/)  
- [PyMongo Cursor Access](https://www.mongodb.com/docs/languages/python/pymongo-driver/current/crud/query/cursors/)   
- [MongoDB sort(), skip(), limit() Docs](https://www.mongodb.com/docs/manual/reference/method/cursor.sort/)  
- [MongoDB batchSize() Docs](https://www.mongodb.com/docs/manual/reference/method/cursor.batchSize/)
