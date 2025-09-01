---

title: MongoDB BSON Types & Document Model
---

#  MongoDB BSON Types & Document Model
---

## 1.1 BSON Value Types Supported by MongoDB

MongoDB stores data as **documents** in a format called **BSON (Binary JSON)**. BSON extends JSON by adding additional data types and optimizing for performance. Unlike plain JSON, which supports only text, numbers, arrays, objects, and boolean, BSON adds precise numerics, dates, binary data, and identifiers.

Understanding BSON types is critical because:

* They affect how queries are written (`ISODate` vs string date).
* Indexes work differently depending on type (`int` vs `double`).
* Some operators (`$type`) directly reference BSON types.

### Example

Let’s create an inventory collection with a variety of BSON-supported fields:

```javascript
db.inventory.insertOne({
  item: "LED Lamp",
  qty: 100,
  price: NumberDecimal("29.99"),     // Decimal128
  inStock: true,                     // Boolean
  tags: ["lighting", "sale"],        // Array
  details: { brand: "BrightLite", warranty: "2 years" },  // Embedded document
  lastUpdated: new Date(),           // Date
  sku: ObjectId()                    // ObjectId
})
```

Now fetch it back in a readable format:

```javascript
db.inventory.findOne({ item: "LED Lamp" })
```

Output will look like:

```json
{
  "_id": ObjectId("64f91c..."),
  "item": "LED Lamp",
  "qty": 100,
  "price": Decimal128("29.99"),
  "inStock": true,
  "tags": ["lighting", "sale"],
  "details": { "brand": "BrightLite", "warranty": "2 years" },
  "lastUpdated": ISODate("2025-09-01T12:00:00Z"),
  "sku": ObjectId("64f91c...")
}
```

Notice the types: `Decimal128`, `Boolean`, `Array`, `Embedded Document`, `Date`, and `ObjectId`.

###  References for Further Study

* [MongoDB Docs – BSON Types](https://www.mongodb.com/docs/manual/reference/bson-types/)
* [MongoDB Docs – Introduction to Documents](https://www.mongodb.com/docs/manual/core/document/)

### Mini-Practice Questions

1. Which BSON type would you use to store precise currency values?
2. True/False: All documents in a MongoDB collection must have the same schema.

---

## 1.2 Flexible Document Model – Co-existence of Different Shapes

MongoDB collections are **schema-flexible**. Unlike SQL tables where every row has the same columns, MongoDB allows documents with **different fields and structures** to live side-by-side.

This flexibility:

* Speeds up development (no schema migrations).
* Supports polymorphic data (e.g., multiple product types in one collection).
* Lets applications evolve naturally.

However, MongoDB also supports **schema validation** if you want to enforce rules.

### Example

Insert three differently shaped documents into the same collection:

```javascript
db.products.insertMany([
  { _id: 1, name: "Basic Chair", price: 49.99 },
  { _id: 2, name: "Boxed Table", price: 129.99, dimensions: { l: 100, w: 60, h: 75 } },
  { _id: 3, name: "Deluxe Sofa", price: 499.99, reviews: [
      { user: "Alice", rating: 5, comment: "Super comfy!" },
      { user: "Bob", rating: 4, comment: "Great value." }
  ]}
])
```

Now query all documents:

```javascript
db.products.find().pretty()
```

Output:

```json
{ "_id": 1, "name": "Basic Chair", "price": 49.99 }
{ "_id": 2, "name": "Boxed Table", "price": 129.99, "dimensions": { "l": 100, "w": 60, "h": 75 } }
{ "_id": 3, "name": "Deluxe Sofa", "price": 499.99, "reviews": [
    { "user": "Alice", "rating": 5, "comment": "Super comfy!" },
    { "user": "Bob", "rating": 4, "comment": "Great value." }
]}
```

Notice how each document has a **different shape**. MongoDB handles them seamlessly.

### References for Further Study

* [MongoDB Docs – Data Modeling Introduction](https://www.mongodb.com/docs/manual/data-modeling/)
* [MongoDB Docs – Schema Validation](https://www.mongodb.com/docs/manual/core/schema-validation/)

### 📝 Mini-Practice Questions

1. Insert a document with a new field `category: "furniture"`. Does MongoDB allow it without altering the existing collection?
2. How would you enforce that all documents in the `products` collection must include a `price` field?

---


