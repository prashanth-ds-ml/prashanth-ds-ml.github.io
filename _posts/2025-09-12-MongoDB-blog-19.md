---

title: MongoDB findAndModify with Concurrent Operations — Behavior and Atomicity

---

# Understanding MongoDB findAndModify with Concurrent Operations

This blog dives deep into the **findAndModify** command in MongoDB, focusing specifically on what happens when it runs concurrently with other operations. This topic is crucial for those preparing for the MongoDB Associate Developer Exam, as well as for developers needing to understand atomicity and concurrency guarantees in real-world applications.

***

## What is findAndModify?

- `findAndModify` is a powerful MongoDB command that atomically finds a document and modifies it (update or remove) in a single operation.
- Commonly used to implement tasks like counters, queues, and atomic updates.
- It combines `find`, `update`, and optionally `remove` into one atomic operation on a single document.

***

## Syntax Overview

```javascript
db.collection.findAndModify({
  query: { <filter> },
  update: { <update operators> },
  new: true,           // Return the updated document
  upsert: false,       // Insert if document not found
  remove: false        // Remove the document instead of updating
})
```

***

## Atomicity and Concurrency Guarantees

- MongoDB guarantees **atomicity at the document level**.
- When multiple concurrent `findAndModify` operations target the same document, MongoDB serializes these writes internally; no partial writes or data corruption occur.
- Each `findAndModify` operation completes fully before the next modifies the document.
- However, the **order of operations** determines which update touches the document last.

***

## Scenario: Concurrent findAndModify Operations

Consider two clients simultaneously running `findAndModify` on the same document:

### Initial Document

```json
{ "_id": 1, "counter": 0 }
```

### Client 1 Operation

```javascript
db.counters.findAndModify({
  query: { _id: 1 },
  update: { $inc: { counter: 1 } },
  new: true
})
```

### Client 2 Operation

```javascript
db.counters.findAndModify({
  query: { _id: 1 },
  update: { $inc: { counter: 1 } },
  new: true
})
```

***

## Possible Outcomes and Database State

- Only one `findAndModify` operation executes at a time on the document.
- The first operation increments `counter` from `0` to `1`.
- The second executes after, incrementing `counter` from `1` to `2`.
- Both clients receive the updated document reflecting the increments.
- The **final document state** always reflects all completed operations.

### Atomicity Summary

| Operation      | Counter Value Returned | Document State After |
|----------------|-----------------------|---------------------|
| Client 1       | 1                     | `counter: 1`        |
| Client 2       | 2                     | `counter: 2`        |

***

## Concurrency with Other Operations

- If another update or write operation occurs concurrently on the same document outside `findAndModify`, atomicity and serialization still hold per document.
- Operations queue on the document lock internally, ensuring **sequential consistency**.
- However, read operations may observe intermediate states if not using read concerns guaranteeing snapshot isolation.

***

## Impact on Application Design

- Use `findAndModify` when you need atomic fetch-and-update semantics, such as unique sequence counters.
- Beware that concurrent changes to other fields may be overwritten unless using update operators carefully.
- For complex multi-document transactions, consider MongoDB multi-document transactions rather than relying solely on `findAndModify`.

***

## PyMongo Equivalent

```python
from pymongo import MongoClient

client = MongoClient("mongodb://localhost:27017/")
db = client.testdb

result = db.counters.find_and_modify(
    query={'_id': 1},
    update={'$inc': {'counter': 1}},
    new=True
)
print("Updated Document:", result)
```

***

## Key Exam and Interview Points

- `findAndModify` performs an atomic find-and-update or find-and-remove on a single document.
- MongoDB serializes concurrent `findAndModify` operations on the same document, preventing race conditions.
- The final database state reflects all completed concurrent operations in some serialized order.
- Use update operators (`$inc`, `$set`) inside `findAndModify` to avoid overwriting unintended fields.
- For atomic multi-document changes, use transactions instead.
- Understanding concurrency and atomicity of `findAndModify` is a common exam topic.

***

## Practice Questions (No Answers)

1. If two concurrent `findAndModify` operations increment a counter field by 1 each, what will the final counter value be?

a) 0  
b) 1  
c) 2  
d) Undefined

2. True or False: `findAndModify` operations on the same document can result in partial writes or corrupted data.

3. Which of the following ensures atomic fetch and update of a single document?

a) Separate `find` and `update` commands  
b) Transaction on multiple collections  
c) `findAndModify` command  
d) Using the `$set` operator in an update

4. When concurrent `findAndModify` operations modify different fields in the same document, what happens?

a) Fields may be overwritten causing data loss.  
b) Operations are serialized, each fully applied in order.  
c) MongoDB throws an error.  
d) Only the first operation succeeds.

***

