---
title: Hands-On MongoDB Guide From Connection to Updates Using Sample Data
---


***

# Hands-On MongoDB Guide: From Connection to Updates Using Sample Data

***

## 1. Connect and List Databases

- Launch **mongosh** shell.
- Run:
  ```js
  show dbs
  ```
- You will see a list of databases including `sample_mflix`.
- Switch to the sample database:
  ```js
  use sample_mflix
  ```
- **Insert screenshots here:**  
![show dbs command](assets/mongodb_blogs/1.show_dbs.png)

***

## 2. Explore Collections in Database

- List all collections in `sample_mflix` to understand data structure:
  ```js
  show collections
  ```
- Observe collections like `movies`, `comments`, `theaters`, etc.
- **Insert screenshots:**  
  Add  and  from Set 2 showing collections listing commands and output.[2][1]

***

## 3. Basic Querying with Filters, Projection, and Limit

- Query movies released after 2000, projecting only `title` and `year`, limiting to 3 documents:
  ```js
  db.movies.find({ year: { $gt: 2000 } }, { title: 1, year: 1 }).limit(3)
  ```
- **Insert screenshot:**  
  Use  (Set 2) showing this query and its results.[3]

- View a full document to understand schema and nested structure:
  ```js
  db.movies.findOne()
  ```
- **Insert screenshot:**  
  Use  (Set 2) showing a full movie document.[4]

***

## 4. Understand Type Sensitivity in Queries

- Queries are strict about types. For example:
  ```js
  db.movies.find({ year: "2000" })
  ```
  Returns no results because `year` is stored as a number, not string.
- Use proper types in filters always.

- **Insert screenshot:**  
  Add  (Set 3) showing empty result for wrong type filter.[3]

***

## 5. Update Documents Safely with `$set`

- Update nested and top-level fields using the `$set` operator:
  ```js
  db.movies.updateOne(
    { title: "Inception" },
    { $set: { "imdb.rating": 9.0, featured: true } }
  )
  ```
- Check update result carefully, noting matched and modified counts.
- Review updated fields using projection:
  ```js
  db.movies.findOne({ title: "Inception" }, { "imdb.rating": 1, featured: 1, _id: 0 })
  ```
- **Insert screenshots:**  
  Use  and  (Set 3) showing the updateOne command and results for success and projection.[1][2]

- Update multiple fields including nested ones:
  ```js
  db.movies.updateOne(
    { title: "Inception" },
    { $set: { "tomatoes.viewer.rating": 4.8, runtime: 158 } }
  )
  ```
- **Insert screenshot:**  
  Add  (Set 3) showing multi-field update and acknowledgement output.[3]

***

## 6. Update Deeply Nested Fields and Arrays Safely

- Updating embedded document fields like awards:
  ```js
  db.movies.updateOne(
    { title: "Inception" },
    { $set: { "awards.text": "won 4 OSCARS. Another 158 wins and 180 nominations" } }
  )
  ```
- Adding to array without duplicates via `$addToSet`:
  ```js
  db.movies.updateOne(
    { title: "Inception" },
    { $addToSet: { genres: "Adventure" } }
  )
  ```
- Update multiple fields including new scalar and string:
  ```js
  db.movies.updateOne(
    { title: "Inception" },
    { $set: { boxofficeUSD: 829895144, plot: "A skilled thief leads a specialized team..." } }
  )
  ```
- **Insert screenshots:**  
  Use , , and  from Set 4 and Set 5 displaying these update operations and their respective outputs.[2][1][3]

***

## 7. Check Update Results and Idempotency

- MongoDB returns JSON summaries for each update. Key fields to verify:
  - `acknowledged`: true means command received.
  - `matchedCount`: number of documents matched.
  - `modifiedCount`: number actually changed.
  - `upsertedCount`: new documents inserted via upsert.
- Running the same update twice may yield a `modifiedCount` of 0 the second time (idempotency).
- Always verify with targeted `findOne` and projections after updates.

- **Insert screenshots:**  
  Use  from Set 4 and  from Set 5 showing update results with `modifiedCount`.[4][1]

***

## Final Practice & Reflection

- Practice `find`, `findOne`, and `updateOne` with nested fields and arrays.
- Use projection often to verify changes and filter out noise.
- Remember type safety in queries: numeric fields must be matched by numbers.
- Re-run updates to notice MongoDB’s idempotency in `modifiedCount`.
- Use `$addToSet` to avoid duplicate array entries.

***
