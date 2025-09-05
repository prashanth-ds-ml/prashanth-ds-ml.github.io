Certainly! Below is a fully enhanced, beginner-friendly, and exam-focused **Hands-On MongoDB Guide** drafted from your content, with clear explanations, practice exercises, key points, and MCQs — all balanced with the corresponding screenshots you provided. This guide is ready for publication or study, aligned with the MongoDB Associate Developer exam topics.

***

# Hands-On MongoDB Guide: From Connection to Updates Using Sample Data

***

## 1. Connect to MongoDB and List Databases

Start by launching the **mongosh** shell and connecting to your MongoDB Atlas cluster.

```js
show dbs
```

This command lists all databases accessible to you, including the sample dataset `sample_mflix`.

Switch to the sample database:

```js
use sample_mflix
```

![Show databases and switch db commands][image2]
![Database switched confirmation][image3]

**Practice Exercise:**

- Try connecting and listing databases in your cluster.
- Switch to another available database and verify with `db`.

***

## 2. Explore Collections in the Sample Database

List all collections in the active database:

```js
show collections
```

You'll see collections such as `comments`, `movies`, `theaters`, etc.

![Show collections command and output][image4]

**Practice Exercise:**

- List collections and pick one (e.g., `movies`).
- Try describing or summarizing what type of data the collection likely contains.

***

## 3. Basic Querying: Filter, Projection, Limit

Query movies released after the year 2000:

```js
db.movies.find({ year: { $gt: 2000 } }, { title: 1, year: 1 }).limit(3)
```

This fetches only the `title` and `year` fields for three movies.

![Query with condition, projection, limit][image5]

Explore a full movie document to understand its structure:

```js
db.movies.findOne()
```

![Retrieve one full movie document][image6]

**Practice Exercise:**

- Modify the query to fetch movies released before 1990.
- Add a field like `imdb.rating` to the projection and observe results.

***

## 4. Query Type Sensitivity

Querying with mismatched data types returns no results.

Example:

```js
db.movies.find({ year: "2000" })
```

No documents are returned because `year` is stored as a number.

Use numbers or correct types in filters:

```js
db.movies.find({ year: 2000 })
```

![No results returned with wrong data type][image7]

**Practice Exercise:**

- Try fetching documents with string vs number for numeric fields.
- Predict which query returns results before running.

***

## 5. Updating Documents Safely with `$set`

Update nested and top-level fields:

```js
db.movies.updateOne(
  { title: "Inception" },
  { $set: { "imdb.rating": 9.0, featured: true } }
)
```

Verify update with projection:

```js
db.movies.findOne({ title: "Inception" }, { "imdb.rating": 1, featured: 1, _id: 0 })
```

![Update with $set and check projection][image8]

Update multiple fields simultaneously:

```js
db.movies.updateOne(
  { title: "Inception" },
  { $set: { "tomatoes.viewer.rating": 4.8, runtime: 158 } }
)
```

![Update multiple fields][image9]

**Practice Exercise:**

- Update `runtime` and add a boolean field `classic: true`.
- Try updating nested fields like `awards.wins`.

***

## 6. Managing Arrays: Safe Addition with `$addToSet`

Add new genres ensuring no duplicates:

```js
db.movies.updateOne(
  { title: "Inception" },
  { $addToSet: { genres: "Adventure" } }
)
```

Safely update deeply nested fields and multi-field sets:

```js
db.movies.updateOne(
  { title: "Inception" },
  { $set: { boxofficeUSD: 829895144, plot: "A skilled thief leads a team expanding the mind." } }
)
```

![Add to set and update multiple fields][image10]
![Add genre safely][image11]

**Practice Exercise:**

- Try adding multiple genres with `$addToSet` using an array.
- Update fields in the embedded `awards` document.
- Observe what happens if the value already exists in the array.

***

## 7. Review Update Results and Idempotency

Each update command returns a result document:

- `acknowledged`: Confirms command reception.
- `matchedCount`: How many documents matched the filter.
- `modifiedCount`: How many documents were changed.
- `upsertedCount`: New documents inserted if `upsert` used.

Repeated updates with the same values yield `modifiedCount: 0`.

**Practice Exercise:**

- Perform update operations twice; note the difference in `modifiedCount`.
- Write queries to verify updated fields.

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

1. Which operator updates specific fields without replacing the entire document?  
a) `$set`  
b) `$replace`  
c) `$push`

2. How do you add an element to an array only if it's not already present?  
a) `$push`  
b) `$addToSet`  
c) `$pull`

3. What does `acknowledged` signify in an update response?  
a) Update failed  
b) Update command was received  
c) Document inserted

4. What would `modifiedCount: 0` typically mean after an update command?  
a) No document matched the filter  
b) Document matched but no data changed  
c) An error occurred during update

5. If a query uses `{ year: "2000" }`, but `year` is stored as a number, what will happen?  
a) Results matching year 2000 are returned  
b) Query returns no documents  
c) Query raises an error

***
