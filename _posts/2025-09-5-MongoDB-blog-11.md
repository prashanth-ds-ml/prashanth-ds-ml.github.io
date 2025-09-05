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
![show dbs command](/assets/mongodb_blogs/1.show_dbs.png)
![use db](/assets/mongodb_blogs/2.use.png)

***

## 2. Explore Collections in Database

- List all collections in `sample_mflix` to understand data structure:
  ```js
  show collections
  ```
- Observe collections like `movies`, `comments`, `theaters`, etc.

![show collections](/assets/mongodb_blogs/3.show_collections.png)
![collections](/assets/mongodb_blogs/3.1.output.png)

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
![find one](/assets/mongodb_blogs/4.findOne.png)
![find one-1](/assets/mongodb_blogs/4.1.findOne.png)

***

## 4. Understand Type Sensitivity in Queries

- Queries are strict about types. For example:
  ```js
  db.movies.find({ year: "2000" })
  ```
  Returns no results because `year` is stored as a number, not string.
- Use proper types in filters always.
![find](/assets/mongodb_blogs/5.find.png)

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


- Update multiple fields including nested ones:
  ```js
  db.movies.updateOne(
    { title: "Inception" },
    { $set: { "tomatoes.viewer.rating": 4.8, runtime: 158 } }
  )
  ```
![update one](/assets/mongodb_blogs/6.updateOne.png)

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
![update one-1](/assets/mongodb_blogs/6.1.updateOne.png)
![update one adToSet](/assets/mongodb_blogs/6.2.updateOne_addToSet.png)

***

## 7. Check Update Results and Idempotency

- MongoDB returns JSON summaries for each update. Key fields to verify:
  - `acknowledged`: true means command received.
  - `matchedCount`: number of documents matched.
  - `modifiedCount`: number actually changed.
  - `upsertedCount`: new documents inserted via upsert.
- Running the same update twice may yield a `modifiedCount` of 0 the second time (idempotency).
- Always verify with targeted `findOne` and projections after updates.

***

## Final Practice & Reflection

- Practice `find`, `findOne`, and `updateOne` with nested fields and arrays.
- Use projection often to verify changes and filter out noise.
- Remember type safety in queries: numeric fields must be matched by numbers.
- Re-run updates to notice MongoDB’s idempotency in `modifiedCount`.
- Use `$addToSet` to avoid duplicate array entries.

***
