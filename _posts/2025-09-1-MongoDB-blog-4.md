---

title: MongoDB BSON Types 
---

# MongoDB BSON Types 

Hey readers! Welcome back to my MongoDB learning series as part of preparing for the **MongoDB Associate Python Developer Exam**.

Today, let's dive deep into **BSON value types supported by MongoDB** — a foundational concept necessary not just for the exam but for practical MongoDB development as well.

***

## What is BSON?

BSON (Binary JSON) is the data format MongoDB uses to store documents. It extends JSON by adding additional data types optimized for performance and flexibility.

Why is understanding BSON important?

- Queries differ based on data types (e.g., querying dates vs strings).
- Indexing behaviors change depending on the type.
- Certain MongoDB operators like `$type` directly use BSON types.
- Using the correct types makes your database efficient and reliable.

***

## Common BSON Value Types and Examples

| BSON Type        | Description                                          | MongoDB Shell Example                                  | PyMongo Example                                         |
|------------------|------------------------------------------------------|-------------------------------------------------------|---------------------------------------------------------|
| **String**       | UTF-8 text, for names, descriptions                  | `{ name: "Alice" }`                                    | `doc = {"name": "Alice"}`                               |
| **Integer**      | Whole numbers                                        | `{ age: 30 }`                                         | `doc = {"age": 30}`                                     |
| **Double**       | Floating point numbers                               | `{ price: 19.99 }`                                    | `doc = {"price": 19.99}`                                |
| **Boolean**      | True or false values                                 | `{ isActive: true }`                                  | `doc = {"isActive": True}`                              |
| **Null**         | Explicit null value                                  | `{ middleName: null }`                                | `doc = {"middleName": None}`                            |
| **Date**         | Date/time in ISO format                              | `{ createdAt: ISODate("2025-09-02T08:00:00Z") }`     | `from datetime import datetime`<br>`doc = {"createdAt": datetime.utcnow()}` |
| **Array**        | Ordered list of items                                | `{ tags: ["mongodb", "python", "database"] }`        | `doc = {"tags": ["mongodb", "python", "database"]}`    |
| **Embedded Doc** | Nested JSON-like document                            | `{ address: { city: "HYD", zip: "500001" } }`        | `doc = {"address": {"city": "HYD", "zip": "500001"}}`  |
| **ObjectId**     | Unique identifier, usually used as the primary key  | `{ _id: ObjectId("507f1f77bcf86cd799439011") }`       | `from bson import ObjectId`<br>`doc = {"_id": ObjectId()}`|
| **Binary Data**  | Binary blobs like files                              | Handled by driver, not shell                           | `from bson.binary import Binary`<br>`doc = {"file": Binary(b"data")}` |
| **Regex**        | Pattern matching                                    | `{ pattern: /example/i }`                             | `import re`<br>`doc = {"pattern": re.compile("example", re.I)}`|

***

## Practical Example: Inserting a Document with Various BSON Types

### MongoDB Shell

```javascript
db.inventory.insertOne({
  item: "LED Lamp",
  qty: 100,
  price: NumberDecimal("29.99"),       // Decimal128 for precise currency
  inStock: true,                       // Boolean to track availability
  tags: ["lighting", "sale"],          // Array of categories
  details: { brand: "BrightLite", warranty: "2 years" }, // Embedded document
  lastUpdated: new Date(),             // Date field
  sku: ObjectId()                      // Unique identifier
})
```

### PyMongo (Python)

```python
from pymongo import MongoClient
from bson import ObjectId
from datetime import datetime
from bson.decimal128 import Decimal128

client = MongoClient("mongodb://localhost:27017/")
db = client.testdb

doc = {
    "item": "LED Lamp",
    "qty": 100,
    "price": Decimal128("29.99"),
    "inStock": True,
    "tags": ["lighting", "sale"],
    "details": {"brand": "BrightLite", "warranty": "2 years"},
    "lastUpdated": datetime.utcnow(),
    "sku": ObjectId()
}

db.inventory.insert_one(doc)
```

***

## Retrieving and Viewing Stored Document

In MongoDB shell:

```javascript
db.inventory.findOne({ item: "LED Lamp" })
```

Expected output shows how BSON types are stored:

```json
{
  "_id": ObjectId("..."),
  "item": "LED Lamp",
  "qty": 100,
  "price": Decimal128("29.99"),
  "inStock": true,
  "tags": ["lighting", "sale"],
  "details": { "brand": "BrightLite", "warranty": "2 years" },
  "lastUpdated": ISODate("2025-09-01T12:00:00Z"),
  "sku": ObjectId("...")
}
```

***

## Key Points (Exam & Interview)

- BSON extends JSON with additional types like `Decimal128`, `ObjectId`, and `Date`.
- The `_id` field uses `ObjectId` by default for unique document IDs.
- Use `Date`/`ISODate` for time-related queries; avoid string dates.
- Arrays and embedded documents support flexible, nested data.
- Proper BSON types impact query behavior and indexing efficiency.

***

## Practice MCQs (No Answers)

1. Which BSON type is best for storing precise monetary values?  
   a) Double  
   b) Decimal128  
   c) String  

2. How does MongoDB represent unique document identifiers internally?  
   a) UUID  
   b) ObjectId  
   c) Random String  

3. What type should be used to store a nested object within a document?  
   a) Array  
   b) Embedded Document  
   c) String  

***
