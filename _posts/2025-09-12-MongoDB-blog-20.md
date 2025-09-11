---
title: Identifying Incorrect Projection Specifications in MongoDB Queries
---

# Understanding Incorrect Projection Identification in MongoDB

This blog explains how to identify incorrect projection specifications in MongoDB queries, an important topic in the MongoDB Associate Developer exam. Projections control which fields or subfields of documents are returned by a query, and improper projection syntax or logic can lead to unexpected results or errors.

***

## What is Projection in MongoDB?

- Projection is used to specify or restrict the fields to return in query results.
- It is the second parameter to the `.find()` or `.findOne()` command.
- Example projection syntax:

```javascript
db.collection.find(
  { <filter> },
  { field1: 1, field2: 1 }
)
```

This returns only `field1` and `field2` for matched documents.

***

## Correct vs Incorrect Projection Specifications

### 1. Including Fields Projection (Inclusion)

- Field values set to `1` or `true` include those fields in the result.
- Example:

```javascript
{ name: 1, age: 1 }
```

### 2. Excluding Fields Projection (Exclusion)

- Field values set to `0` or `false` exclude those fields.
- Example:

```javascript
{ password: 0, ssn: 0 }
```

### Important Rule:

- You **cannot mix inclusion and exclusion** in the same projection document (except exclusion of `_id`).
- For example, `{ name: 1, password: 0 }` is invalid and causes errors.

***

## Common Incorrect Projection Pitfalls

| Error Type                        | Example                       | Explanation                                     |
|----------------------------------|-------------------------------|------------------------------------------------|
| Mixing inclusion and exclusion    | `{ name: 1, password: 0 }`     | MongoDB disallows mixing include and exclude except for `_id` |
| Using non-boolean values          | `{ name: "yes", age: 123 }`    | Projection values must be `1`, `0`, `true`, or `false` |
| Including and excluding `_id`     | `{ _id: 0, _id: 1 }`           | `_id` can be excluded or included, but not both simultaneously |
| Using nested projections incorrectly | `{ "address.city": 0, address: 1 }` | Conflicting projection on parent and child fields |

***

## Examples of Incorrect Projections

### Mixing Inclusion and Exclusion Except `_id`

```javascript
db.users.find({}, { name: 1, password: 0 })
```

**Error:** Cannot specify inclusion (`name:1`) and exclusion (`password:0`) in same projection.

***

### Non-Boolean Projection Values

```javascript
db.users.find({}, { name: "yes" })
```

**Error:** Projection value must be boolean or 1/0.

***

### Conflicting Nested Projections

```javascript
db.orders.find({}, { "shipping.address": 1, shipping: 0 })
```

**Error:** Cannot include child field (`shipping.address`) while excluding parent (`shipping`).

***

## Correct Projection Examples

### Include Specific Fields

```javascript
db.users.find({}, { name: 1, email: 1, _id: 0 })
```

Returns only `name` and `email` fields excluding `_id`.

### Exclude Sensitive Fields

```javascript
db.users.find({}, { password: 0, ssn: 0 })
```

Excludes `password` and `ssn` but includes all other fields, including `_id`.

### Nested Field Projection

```javascript
db.orders.find({}, { "shipping.address": 1, _id: 0 })
```

Returns only the nested `shipping.address` field.

***

## PyMongo Projection Syntax

```python
cursor = db.users.find(
    {},
    {"name": 1, "email": 1, "_id": 0}
)
for doc in cursor:
    print(doc)
```

***

## Key Points for Exam and Interviews

- In projections, **do not mix inclusion and exclusion**, except excluding `_id`.
- Projection values must be boolean or 1/0.
- Conflicting projections on parent and child paths cause errors.
- Properly constructed projections improve query performance by returning only necessary data.
- Incorrect projections may silently cause unexpected results or explicit errors.
- Always test projections to ensure correct fields are returned.

***

## Practice Questions (No Answers)

1. Which of the following is an invalid projection?

a) `{ name: 1, age: 1 }`  
b) `{ password: 0, ssn: 0 }`  
c) `{ name: 1, password: 0 }`  
d) `{ name: true, email: true }`  

2. What will happen if you use non-boolean values in projection like `{ name: "yes" }`?

a) Field `name` is included  
b) MongoDB throws an error  
c) Field `name` is excluded  
d) Projection is ignored  

3. Can you exclude the `_id` field while including other fields in projection?

a) Yes  
b) No  

4. Is the following projection valid? `{ "address.city": 1, address: 0 }`

a) Yes  
b) No  

***
