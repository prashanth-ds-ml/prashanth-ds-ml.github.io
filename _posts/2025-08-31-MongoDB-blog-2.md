---
title: "MongoDB: Connecting with Mongosh & Creating Your First DB, Collection, and Document"
---

# MongoDB: Connecting with Mongosh & Creating Your First DB, Collection, and Document

Hey readers, this blog is part of my learning journey while preparing for the **MongoDB Associate Python Developer Exam**.  
Today, let’s start simple — connecting to MongoDB with **mongosh**, exploring databases, and creating our first **DB → collection → document**.

---

##  Step 1: Get your connection URI  

Head to your **MongoDB Atlas** dashboard → click **Clusters** → **Connect** → **Connect with Mongosh**.  
You’ll see a URI like this:

```bash
mongosh "mongodb+srv://<cluster>.mongodb.net/" --apiVersion 1 --username <user>
````

---

##  Step 2: Connect to Mongosh

Open your terminal and paste the URI. Enter the password you set for the database user:

```bash
mongosh "mongodb+srv://cluster0.xq0bvx8.mongodb.net/" --apiVersion 1 --username myUser
```

If successful, you’ll see a welcome message and a `>` prompt.

---

##  Step 3: Show existing databases

```javascript
show dbs
```

This lists all databases in the cluster.

---

##  Step 4: Check the current DB

```javascript
db
```

By default, this shows `test`.

---

##  Step 5: Create a new database

```javascript
use blog_demo
```

This switches context to `blog_demo`.
Remember: MongoDB **creates the DB only when you add data**.

---

##  Step 6: Create a collection

A **collection** is like a folder inside a database. It groups related **documents** (like a table in SQL, but more flexible).

Example:

```javascript
db.createCollection("students")
```

Or, MongoDB can auto-create a collection when you insert the first document.

---

##  Step 7: Create a document

A **document** is the basic unit of data in MongoDB, stored in JSON-like structure (internally BSON).

Example:

```javascript
db.students.insertOne({
  name: "Meera",
  year: 1,
  gpa: 8.9,
  enrolled: true,
  skills: ["python", "sql"],
  address: { city: "Hyderabad", pincode: 500001 }
})
```

MongoDB will automatically add an `_id` field to uniquely identify this document.

---

## Understanding Collections, Documents, and Objects

Now that you’ve run your first insert, let’s pause and understand the concepts:

* **Database** → logical container of collections (like a database in SQL).
* **Collection** → a grouping of related documents (like a table, but schema-flexible).
* **Document** → a single record, made of key–value pairs in JSON-like format.
* **Objects/Fields** → inside documents, fields can hold strings, numbers, booleans, arrays, or even nested objects.

Example:

```javascript
{
  "_id": ObjectId("66d7cfd8b234..."), 
  "name": "Meera",
  "year": 1,
  "gpa": 8.9,
  "enrolled": true,
  "skills": ["python", "sql"],
  "address": {
    "city": "Hyderabad",
    "pincode": 500001
  }
}
```

Notice:

* `_id` is auto-generated.
* `skills` is an **array**.
* `address` is a **nested object**.

This flexibility makes MongoDB different from traditional databases.

---

## 🧾 SQL vs MongoDB Analogy

| Concept      | SQL (Relational)   | MongoDB (Document)  |
| ------------ | ------------------ | ------------------- |
| Database     | Database           | Database            |
| Table        | Table              | Collection          |
| Row          | Row                | Document            |
| Column/Field | Column (fixed)     | Field (dynamic)     |
| Primary Key  | `id`               | `_id` (auto)        |
| Nested Data  | Not natural (JOIN) | Native (arrays/obj) |

