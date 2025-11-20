
---

# ✅ **SECTION 1.1 — Embedded vs Referenced Documents (Clear, Simple, Exam-Friendly)**

*(8% of exam — very easy to score if understood properly)*

---

# 🔷 **What Are Embedded Documents?**

Embedded documents are **sub-documents stored directly inside a parent document**.

Think of them like storing everything **inside one JSON object**.

### ⭐ When to use embedded data:

* When the child data **always belongs** to the parent
* When it is **always retrieved together** with the parent
* When the relationship is **1-to-few**
* When you want **fast reads** (no joins)

### ✔ Example (from sample_mflix.movies):

```json
{
  "title": "In the Land of the Head Hunters",
  "imdb": {
    "rating": 5.8,
    "votes": 223
  },
  "awards": {
    "wins": 1,
    "nominations": 0
  }
}
```

Here, `imdb` and `awards` are **embedded**.

They live **inside** the movie document.

### 🎯 Why embed?

Because reading a movie should also give you its rating and awards immediately — this makes reads **very fast**, without joins.

---

# 🔷 **What Are Referenced Documents?**

Referenced documents are stored in a **different collection**, and you link them using an `_id`.

Think of it like relational DB foreign keys.

### ⭐ When to use referencing:

* When the relationship is **1-to-many** or **many-to-many**
* When child data is **large**
* When child data is **shared between many parents**
* When child data changes frequently
* When data shouldn’t be duplicated

### ✔ Example (from sample_mflix):

Inside `comments` collection:

```json
{
  "movie_id": ObjectId("573a1390f29313caabcd56df"),
  "text": "Amazing movie!"
}
```

This points to a movie in:

```js
db.movies.findOne({ _id: movie_id })
```

---

# 🔷 **Embedded vs Referenced — Clear Intuition**

Imagine a **Movie** that has **tiny metadata** like:

* rating
* votes
* awards

→ All of these are small & always needed → **EMBED**

But imagine a **Movie** with:

* 5,000 comments
* each comment 1 KB of text

→ Storing all inside the movie would make the doc huge, slow → **REFERENCE**

---

# 🟢 **Hands-On Using sample_mflix**

### 1️⃣ View Embedded Data (imdb, awards, tomatoes)

```js
db.movies_working.findOne(
  {},
  { title: 1, imdb: 1, awards: 1, tomatoes: 1, _id: 0 }
)
```

---

### 2️⃣ View Referenced Data

One comment:

```js
const c = db.comments.findOne()
c
```

Find referenced movie:

```js
db.movies.findOne({ _id: c.movie_id })
```

---

### 3️⃣ View All Movies with Embedded Awards

```js
db.movies.find({ awards: { $exists: true } })
```

---

### 4️⃣ Find Comments Written by a Specific User

```js
db.comments.find({ email: c.email })
```

This shows “referenced relationships” using fields like `movie_id` and `email`.

---

# 🧪 **YOUR PRACTICE TASKS (Write commands yourself)**

No MCQs yet — these are task-style prompts.

### **Task 1**

Find the embedded `viewer` object inside `tomatoes` for any movie.

```js
db.movies.findOne(
  { "tomatoes.viewer": { $exists: true } },
  { title: 1, "tomatoes.viewer": 1, _id: 0 }
)
```
```js
db.movies.find(
  { "tomatoes.viewer": { $exists: true } },
  { title: 1, "tomatoes.viewer": 1, _id: 0 }
).limit(5)

```

### **Task 2**

Pick a random comment (`db.comments.findOne()`) and fetch its parent movie.
---

## 🧩 Step 1 – Pick a random comment

```js
use sample_mflix

const c = db.comments.findOne()
c
```

Look at what you get back. You should see something like:

```json
{
  "_id": ObjectId("..."),
  "name": "Some User",
  "email": "user@example.com",
  "movie_id": ObjectId("573a1390f29313caabcd56df"),
  "text": "Nice movie!",
  "date": ISODate("2012-03-12T00:00:00Z")
}
```

Key thing:
👉 `movie_id` is a **reference** to a document in the `movies` collection.

---

## 🎯 Step 2 – Fetch its parent movie

Use `movie_id` to look up the movie:

```js
db.movies.findOne({ _id: c.movie_id })
```

Optionally, limit fields to see it cleaner:

```js
db.movies.findOne(
  { _id: c.movie_id },
  { title: 1, year: 1, imdb: 1, _id: 0 }
)
```

You’ve just done a **manual join in two queries**:

* First query: `comments` → get `movie_id`
* Second query: `movies` → fetch parent movie by `_id`

This is exactly how **referencing** works in MongoDB.

---

### **Task 3**

Find all movies that have an embedded `tomatoes.production` field.

We want movies where:

* `tomatoes` exists
* and inside it, the field `production` exists

In MongoDB, the right way is to use **dot notation + `$exists`**.

---

## ✅ Correct query

```js
db.movies.find(
  { "tomatoes.production": { $exists: true } },
  { title: 1, "tomatoes.production": 1, _id: 0 }
)
```

Explanation:

* `"tomatoes.production"` → go inside the `tomatoes` embedded document and look for the `production` field
* `{ $exists: true }` → only match documents where that field exists
* Projection `{ title: 1, "tomatoes.production": 1, _id: 0 }` → show only the movie title and its `tomatoes.production` value

You can also just check one:

```js
db.movies.findOne(
  { "tomatoes.production": { $exists: true } },
  { title: 1, "tomatoes.production": 1, _id: 0 }
)
```

---

If you want, you can now try to tweak it:

* Show only movies where `tomatoes.production` contains `"Corporation"` (hint: use `$regex`)
* Or find how many such movies exist (hint: `.countDocuments()`)

### **Task 4**

List all distinct `movie_id` values in the comments collection.

MongoDB gives a built-in method for this:

```js
db.comments.distinct("movie_id")
```

This returns an array of all unique `movie_id` values referenced in the comments collection.

These values correspond to `_id` values in the `movies` collection.

---

# ⭐ Optional: View only first few values nicely

```js
db.comments.distinct("movie_id").slice(0, 10)
```

---

# ⭐ Optional: Count how many unique movie_ids exist

```js
db.comments.distinct("movie_id").length
```
### **Task 5**

Find 3 movies that have both:

* embedded `imdb.rating`
* embedded `awards.wins`

We want movies where:

* `imdb.rating` exists
* `awards.wins` exists

---

## ✅ Correct query

```js
db.movies.find(
  {
    "imdb.rating": { $exists: true },
    "awards.wins": { $exists: true }
  },
  {
    title: 1,
    "imdb.rating": 1,
    "awards.wins": 1,
    _id: 0
  }
).limit(3)
```

### Why this works:

* `"imdb.rating": { $exists: true }`
  → Only match docs where the embedded field `rating` exists inside `imdb`.

* `"awards.wins": { $exists: true }`
  → Only match docs where the embedded field `wins` exists inside `awards`.

* Both conditions in the same object → **logical AND**.

* Projection `{ title: 1, "imdb.rating": 1, "awards.wins": 1, _id: 0 }`
  → Just show what we care about.

---

## 🔁 Small variations you can try

1. **Filter for good rating on top of existence:**

   ```js
   db.movies.find(
     {
       "imdb.rating": { $gte: 8.0 },
       "awards.wins": { $gte: 1 }
     },
     { title: 1, "imdb.rating": 1, "awards.wins": 1, _id: 0 }
   ).limit(3)
   ```

2. **Sort by number of awards won:**

   ```js
   db.movies.find(
     {
       "imdb.rating": { $exists: true },
       "awards.wins": { $exists: true }
     },
     { title: 1, "imdb.rating": 1, "awards.wins": 1, _id: 0 }
   )
   .sort({ "awards.wins": -1 })
   .limit(3)
   ```

These patterns will be super useful later when we do **relational operators (`$gt`, `$gte`) and logical operators (`AND` behaviour)**.

---

# 🔥 **5 EXAM-STYLE QUESTIONS (NO ANSWERS)**

I’ll mark each question as **Single-Select** or **Multi-Select** exactly like the real exam.

### **Q1 — Single Select**

Which of the following best describes *embedded* documents?

A. Storing small related data inside the parent document
B. Storing related data in a separate collection
C. Using foreign keys to reference other documents
D. Using `$lookup` to join data across collections

---

### **Q2 — Multi Select**

In which situations is **referencing** recommended?

A. Many-to-many relationships
B. Very large child data
C. Always-read-together data
D. Data shared among many parent documents

---

### **Q3 — Single Select**

Which operation requires **referenced** data instead of embedded?

A. Increasing a movie’s IMDb vote count
B. Fetching all comments for a movie
C. Accessing a movie’s runtime
D. Reading movie languages

---

### **Q4 — Multi Select**

Which are advantages of **embedded** documents?

A. Faster reads
B. Atomic updates
C. Lower storage cost
D. Less duplication

---

### **Q5 — Single Select**

Which of the following is true?

A. Embedded documents reduce the number of queries required
B. Referenced documents always load faster
C. MongoDB automatically joins referenced relationships
D. Embedded documents force schema validation

---


