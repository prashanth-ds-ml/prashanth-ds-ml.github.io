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

## Key Points for Exam and Interviews

- MongoDB uses BSON types; querying with mismatched types yields no results.
- The `$set` operator updates specified fields without overwriting entire documents.
- Use dot notation for nested and embedded document updates.
- `$addToSet` safely updates arrays without duplicates.
- Always check update result fields (`matchedCount`, `modifiedCount`) to confirm actions.
- MongoDB updates are idempotent: no changes if values are identical.
- Use projections to verify data without fetching entire documents.
- Properly writing queries with correct syntax and understanding outcomes is critical for exams and real-world applications.

***

## Sample Practice Questions (Write your own queries)

1. Write a query to find all movies directed by "Christopher Nolan" released after 2010.

2. Write an update to add a new field `availableOnStreaming: true` to movies with less than 100 minutes runtime.

3. Update the `tomatoes.viewer.rating` for "Inception" to 4.9.

4. Add "Drama" and "Thriller" genres to a movie ensuring no duplicates.

5. Retrieve only the `title`, `year`, and `imdb.rating` fields for movies released before 1990.

***

## Multiple Choice Questions (No answers provided, for self-assessment)

**Q1:**. Which operator updates specific fields without replacing the entire document?  
a) `$set`  
b) `$replace`  
c) `$push`

**Q2:** How do you add an element to an array only if it's not already present?  
a) `$push`  
b) `$addToSet`  
c) `$pull`

**Q3:** What does `acknowledged` signify in an update response?  
a) Update failed  
b) Update command was received  
c) Document inserted

**Q4:** What would `modifiedCount: 0` typically mean after an update command?  
a) No document matched the filter  
b) Document matched but no data changed  
c) An error occurred during update

**Q5:** If a query uses `{ year: "2000" }`, but `year` is stored as a number, what will happen?  
a) Results matching year 2000 are returned  
b) Query returns no documents  
c) Query raises an error

**Q6:** Which of the following queries will correctly find all movies released after the year 2000?

a) `db.movies.find({ year: { $gt: 2000 } })`  
b) `db.movies.find({ year: "2000" })`  
c) `db.movies.find({ year: { $lt: 2000 } })`  
d) `db.movies.find({ year: { $gte: "2001" } })`

***

**Q7:** Consider the query:  
`db.movies.find({ "imdb.rating": 8.5 })`  
What happens if the `imdb.rating` field is stored as a string instead of a number?

a) The query returns matching documents anyway.  
b) The query returns no documents due to type mismatch.  
c) The query throws an error.  
d) The query converts the type implicitly and returns matches.

***

**Q8:** Which of the following query expressions will cause no results even if a matching document exists?

a) `db.movies.find({ year: 2010 })`  
b) `db.movies.find({ year: "2010" })`  
c) `db.movies.find({ year: { $eq: 2010 } })`  
d) `db.movies.find({ "imdb.rating": { $gt: 7 } })`

***

**Q9:** What is wrong with the following update command?

```js
db.movies.updateOne(
  { title: "Inception" },
  { rating: 9 }
)
```

a) It will only update the `rating` field.  
b) It will replace the entire document with `{ rating: 9 }`.  
c) It will throw a syntax error.  
d) It will add a nested `rating` field.

***

**Q10:** After running this update, what does `modifiedCount: 0` indicate?

```js
db.movies.updateOne(
  { title: "Inception" },
  { $set: { featured: true } }
)
```

a) No documents matched the filter.  
b) Documents matched but the `featured` field was already `true`.  
c) The update failed due to syntax error.  
d) A new document was inserted.

***

**Q11:** What will happen if you execute the following twice?

```js
db.movies.updateOne(
  { title: "Inception" },
  { $addToSet: { genres: "Adventure" } }
)
```

a) The `Adventure` genre will be added twice.  
b) The second update will add `Adventure` again.  
c) The second update will make no changes as `Adventure` already exists.  
d) The command will fail the second time.

***


**Q12:** What is the effect of the following query projection?

```js
db.movies.find({ title: "Inception" }, { _id: 0, title: 1, runtime: 1 });
```

a) Returns complete documents with `_id` excluded.  
b) Returns documents with only `title` and `runtime` fields, excluding `_id`.  
c) Returns documents only with `title` field.  
d) Returns documents excluding all fields.

***

**Q13:** Which of these projection specifications is invalid?

a) `{ title: 1, runtime: 1 }`  
b) `{ title: 1, runtime: 0 }`  
c) `{ title: 1, runtime: true }`  
d) `{ title: 0, runtime: 0 }`

***


**Q14:** Which command will insert a new document if no document matches the filter criteria?

a) `db.movies.updateOne(filter, update)`  
b) `db.movies.updateOne(filter, update, { upsert: true })`  
c) `db.movies.insertOne(document)`  
d) `db.movies.save(document)`

***

**Q15:** What is the purpose of the `$set` operator in an update command?

a) Replace the entire document with the specified fields.  
b) Update the specified fields or add them if they do not exist.  
c) Remove specified fields from the document.  
d) Append values to array fields unconditionally.

***
