---

title: MongoDB Aggregation Expressions Using $lookup and $out

---

# Understanding MongoDB Aggregation Stages: $lookup and $out

This blog focuses on two powerful MongoDB aggregation pipeline stages: **$lookup** and **$out**, aligned with exam topics 2.21 and 2.22. While basic aggregation such as `$match`, `$group`, and `$sort` are commonly used, `$lookup` enables joins between collections and `$out` allows writing aggregation results to collections, making them essential tools for real-world analytics and ETL workflows.

***

## The `$lookup` Stage — Performing Joins Between Collections

### Purpose

- `$lookup` performs a left outer join to combine documents from one collection with related documents from another collection.
- It enriches documents with data from a different (foreign) collection based on a specified field.

### Basic Syntax

```js
{
  $lookup: {
    from: "<foreign_collection>",
    localField: "<local_field>",
    foreignField: "<foreign_field>",
    as: "<output_array_field>"
  }
}
```

- `from`: The collection to join with.
- `localField`: The field in the input documents.
- `foreignField`: The field in the `from` collection to match.
- `as`: The name of the new array field to store joined results.

***

### Example: Joining Orders with Customers

Assume two collections: `orders` and `customers`.

- Each order has a `customerId`.
- Each customer has an `_id`.

Aggregation:

```js
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customerId",
      foreignField: "_id",
      as: "customer_info"
    }
  }
])
```

Result:

Each order document will contain an array `customer_info` with matching customer documents.

***

### Advanced `$lookup`

- Support for pipeline syntax to filter or project foreign documents.
- Allows joins on multiple fields or complex conditions (introduced in MongoDB 3.6+).
- Example with pipeline lookup:

```js
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      let: { custId: "$customerId" },
      pipeline: [
        { $match: { $expr: { $eq: ["$_id", "$$custId"] } } },
        { $project: { name: 1, email: 1 } }
      ],
      as: "customer_info"
    }
  }
])
```

***

## The `$out` Stage — Writing Aggregation Results to a Collection

### Purpose

- `$out` writes the output of an aggregation pipeline to a given collection.
- Useful for creating materialized views or exporting transformed data.
- The target collection is **overwritten** (replaced) by the aggregation output.

### Basic Syntax

```js
{
  $out: "<output_collection_name>"
}
```

***

### Example: Storing Grouped Movie Counts by Year

```js
db.movies.aggregate([
  { $group: { _id: "$year", totalMovies: { $sum: 1 } } },
  { $out: "movies_by_year" }
])
```

- Creates or replaces the `movies_by_year` collection.
- Each document contains a year and the count of movies for that year.

***

### Important Notes on `$out`

- The operation will fail if the user lacks write permission for the target collection.
- `$out` must be the **last stage** in the aggregation pipeline.
- The target collection is replaced atomically for consistent results.
- Outputs an empty cursor because the stage writes directly to the collection.

***

## Using `$lookup` and `$out` Together

Example: Join, transform, and save

```js
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customerId",
      foreignField: "_id",
      as: "customer_info"
    }
  },
  {
    $match: { status: "shipped" }
  },
  {
    $out: "shipped_orders_with_customer"
  }
])
```

This pipeline joins orders with customers, filters shipped orders, then stores results in a new collection.

***

## PyMongo Example of `$lookup` and `$out`

```python
from pymongo import MongoClient

client = MongoClient("mongodb://localhost:27017/")
db = client.sample_mflix

pipeline = [
    {
        "$lookup": {
            "from": "comments",
            "localField": "_id",
            "foreignField": "movie_id",
            "as": "comments"
        }
    },
    {
        "$out": "movies_with_comments"
    }
]

db.movies.aggregate(pipeline)
```

This joins movies with their comments and writes to `movies_with_comments` collection.

***

## Key Points for Exam and Interviews

- `$lookup` enables join-like operations in MongoDB aggregations.
- Joins return arrays of matching documents in the specified output field.
- `$out` writes aggregation results to a collection, replacing its contents atomically.
- `$out` must be the last stage in an aggregation pipeline.
- Using these stages efficiently supports reporting, ETL, and analytical workloads.
- Understanding syntax and use cases is critical for certification exams.

***

## Practice Questions (No Answers)

1. What does the `$lookup` stage do in an aggregation pipeline?  
a) Filters documents in current collection  
b) Joins with another collection based on matching fields  
c) Groups documents by a field  

2. Which of the following fields is required in `$lookup`?  
a) `localField`  
b) `foreignField`  
c) `from`  
d) All of the above  

3. What effect does the `$out` stage have on the output collection?  
a) Adds documents without removing existing data  
b) Overwrites the existing collection atomically  
c) Only updates existing documents  

4. Where must the `$out` stage appear in the aggregation pipeline?  
a) Anywhere  
b) At the last position  
c) At the first position  

***
