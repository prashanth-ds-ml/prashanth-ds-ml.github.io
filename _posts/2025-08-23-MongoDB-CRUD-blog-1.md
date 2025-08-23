---
title: "MongoDB CRUD — Lesson 1: `insertOne()` & `insertMany()` (lite, clear, and to the point)"
---

# MongoDB CRUD — Lesson 1: `insertOne()` & `insertMany()` (lite, clear, and to the point)

You’re kicking off CRUD with **Create**. In this lesson, MongoDB University shows how to add data in **mongosh** using two methods: one for a single document and one for a batch.

---

## `insertOne()` — Syntax → then an example

### Syntax (mongosh)

```javascript
db.<collectionName>.insertOne(<document>)
````

* **`<collectionName>`**: the collection you’re writing to (e.g., `inventory`).
* **`<document>`**: a JSON-like object with your fields (e.g., `{ item: "pen", qty: 20 }`).

**What you get back:** an object that includes

* `acknowledged` — whether the server confirmed the write, and
* `insertedId` — the `_id` of the newly inserted document.

### Example

```javascript
db.inventory.insertOne({ item: "pen", qty: 20 })
```

After running it, check the result: you should see `acknowledged: true` and an `insertedId` value.

---

## `insertMany()` — Syntax → then an example

### Syntax (mongosh)

```javascript
db.<collectionName>.insertMany([ <document1>, <document2>, ... ])
```

* Pass **an array** of documents (each one looks like the single-doc example above).

**What you get back:** an object that includes

* `acknowledged`, and
* `insertedIds` — an **array-like mapping** of one id per inserted document.

### Example

```javascript
db.inventory.insertMany([
  { item: "notebook", qty: 50 },
  { item: "stapler",  qty: 10 },
  { item: "marker",   qty: 12 }
])
```

Check the result for `acknowledged: true` and **`insertedIds`** containing an id for each of the three documents.


