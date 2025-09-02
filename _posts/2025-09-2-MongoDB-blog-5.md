---

title: MongoDB Document Model — Co-existence of Differently Shaped Documents in a Collection
---

***

# MongoDB Document Model — Co-existence of Differently Shaped Documents in a Collection

Hey readers! Welcome back to my MongoDB learning series as part of preparing for the **MongoDB Associate Python Developer Exam**.

Today, we discuss how MongoDB collections support documents with different structures, a key feature that provides great flexibility compared to traditional relational databases.

***

## MongoDB Collections Are Schema-Flexible

Unlike SQL tables where every row has the same fixed columns, MongoDB lets you store documents with different fields and shapes in the same collection. This is called **schema flexibility**.

### Why Is This Useful?

- No need for expensive schema migrations during development or filtering out unused columns.
- Supports multiple variants of data in one place (e.g., products, events, user profiles).
- Allows your application’s data structure to evolve naturally over time.

***

## Example: Inserting Different Document Shapes in One Collection

Suppose we want to store different product types in a collection named `products`.

```javascript
db.products.insertMany([
  { _id: 1, name: "Basic Chair", price: 49.99 },
  { _id: 2, name: "Boxed Table", price: 129.99, dimensions: { length: 100, width: 60, height: 75 } },
  { _id: 3, name: "Deluxe Sofa", price: 499.99, reviews: [
      { user: "Alice", rating: 5, comment: "Super comfy!" },
      { user: "Bob", rating: 4, comment: "Great value." }
  ]}
])
```

Each document has:

- Different fields (`dimensions` in one, `reviews` in another, none in the first).
- Different nested structures (objects and arrays).
- All coexist happily in the same `products` collection.

***

## Query All Documents

Running:

```javascript
db.products.find().pretty()
```

Returns:

```json
{ "_id": 1, "name": "Basic Chair", "price": 49.99 }
{ "_id": 2, "name": "Boxed Table", "price": 129.99, "dimensions": { "length": 100, "width": 60, "height": 75 } }
{ "_id": 3, "name": "Deluxe Sofa", "price": 499.99, "reviews": [
    { "user": "Alice", "rating": 5, "comment": "Super comfy!" },
    { "user": "Bob", "rating": 4, "comment": "Great value." }
]}
```

***

## PyMongo Example: Insert Documents of Different Shapes

```python
from pymongo import MongoClient

client = MongoClient("mongodb://localhost:27017/")
db = client.testdb

products = [
    {"_id": 1, "name": "Basic Chair", "price": 49.99},
    {"_id": 2, "name": "Boxed Table", "price": 129.99,
     "dimensions": {"length": 100, "width": 60, "height": 75}},
    {"_id": 3, "name": "Deluxe Sofa", "price": 499.99,
     "reviews": [
         {"user": "Alice", "rating": 5, "comment": "Super comfy!"},
         {"user": "Bob", "rating": 4, "comment": "Great value."}
     ]}
]

db.products.insert_many(products)
```

***

## Important Notes

- **Schema-less collections do NOT enforce uniformity**, which speeds up development but requires careful validation in app logic or optional MongoDB JSON Schema validation.
- Inconsistent data shapes can make querying complex, so plan your queries accordingly.
- MongoDB supports **schema validation** if uniformity is desired later during production.

***

## Key Points (For Exam & Interviews)

- MongoDB collections hold documents with **different fields and structures** simultaneously.
- This flexibility distinguishes MongoDB from relational databases (no fixed schema).
- Schema validation can be set but is optional.
- Nested and array fields allow storing complex data, enabling natural modeling of real-world objects.

***

## Practice MCQs (No Answers)

1. Can MongoDB store documents of totally different shapes within the same collection?  
   a) Yes  
   b) No  

2. What is the main benefit of MongoDB’s schema flexibility?  
   a) Requires schema migrations for every change  
   b) Supports polymorphic data and quicker development  
   c) Enforces strict data types  

3. How can MongoDB enforce uniform document structure if needed?  
   a) By installing plugins  
   b) By using JSON Schema Validation  
   c) By default, all collections are uniform  

***

