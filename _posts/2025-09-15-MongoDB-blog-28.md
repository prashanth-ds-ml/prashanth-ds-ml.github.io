---
title: MongoDB Error Handling Patterns — Production Safety and Exam Essentials
---

# Introduction

This blog explores robust error handling patterns for MongoDB operations using both mongosh and PyMongo, a topic vital for production reliability and frequently covered in certification and interviews. Proper error management covers handling connection issues, operational failures (like duplicate keys or validation errors), and responding with actionable error messages or recovery steps.

# Syntax Breakdown

### 1. Error Handling in mongosh (JavaScript)

```javascript
try {
  db.users.insertOne({ _id: 1, name: "Asha" });
} catch (e) {
  print("Error:", e.message);
  // Optionally inspect: e.code, e.stack, e
}
```

- Use `try...catch` for handling expected or unexpected errors in the shell.[1][2]

### 2. Error Handling in PyMongo (Python)

```python
from pymongo import MongoClient, errors

try:
    client = MongoClient("mongodb://localhost:27017/", serverSelectionTimeoutMS=5000)
    db = client.testdb
    result = db.users.insert_one({"_id": 1, "name": "Asha"})
except errors.DuplicateKeyError:
    print("Duplicate _id detected! Document not inserted.")
except errors.ServerSelectionTimeoutError:
    print("Could not connect to MongoDB server.")
except errors.PyMongoError as e:
    print("General error occurred:", e)
```

- Catch specific exceptions like `DuplicateKeyError` for precise troubleshooting and global `PyMongoError` for fallback.[3][4][5]

# Explanation of Usage

- **Catch Specific Errors:** Always catch the most specific exception first (e.g., DuplicateKeyError, ServerSelectionTimeoutError) to give users helpful feedback or execute custom recovery logic.
- **Connection Handling:** Use timeouts and catch connection exceptions to detect and deal with server outages programmatically.
- **Operational Failures:** Handle classic issues (duplicate keys, validation, missing fields, and write conflicts) with tailored error messages, possibly prompting user correction.
- **Custom Handling:** For critical errors, implement retries, circuit breakers, alerting, or maintenance modes as needed.[6][7]

# Practical Examples

## mongosh: Handle Validation Error

```javascript
try {
  db.users.insertOne({ _id: 1, email: "not-an-email" })  // validation might fail
} catch (e) {
  if (e.code === 121) {
    print("Document validation failed:", e.message);
  } else {
    print("Other error:", e.message);
  }
}
```

## PyMongo: Handle Multiple Error Cases

```python
from pymongo.errors import DuplicateKeyError, WriteError, OperationFailure

try:
    # Suppose unique index on email
    db.users.insert_one({"_id": 1, "email": "test@example.com"})
except DuplicateKeyError:
    print("A user with that ID or email already exists.")
except WriteError as we:
    print("A write operation error:", we)
except OperationFailure as oe:
    print("Operation failed:", oe)
```

# Common MongoDB Error Codes

| Code/Exception                | Meaning                         |
|-------------------------------|---------------------------------|
| 11000 (DuplicateKeyError)     | Duplicate unique key on insert  |
| 121 (DocumentValidation)      | Fails schema/doc validation     |
| PyMongoError                  | Generic pymongo error           |
| ServerSelectionTimeoutError   | Connection/server unavailable   |
| OperationFailure              | General command/ops failure     |

Find full error code details in [MongoDB Error Codes Documentation].[8]

# Best Practices

- Catch the most specific error possible first, then more general.
- Provide user-friendly error messages and log raw exceptions for developers.
- Log/retry transient errors (e.g., network blips, timeouts).
- Use validators in schemas to prevent invalid data errors.
- Include error codes in API responses to help frontend/client troubleshooting.
- For mission critical workflows, implement alerting and monitoring on error spikes.

# Key Points for Exam and Interviews

- Explain structured error handling using try/catch or try/except in JS and Python.
- Know common MongoDB error codes and exceptions, especially for unique index and connection failures.
- Error handling logic is often a real-world coding/discussion test area for backend roles and database admin.
- Clean, well-logged, and user-friendly error responses prevent confusion, outages, and data loss.

# Practice Questions (MCQs) — No Answers

1. Which of the following exceptions is raised on duplicate unique key insert in PyMongo?  
a) WriteError  
b) DuplicateKeyError  
c) OperationFailure  

2. How should you handle a connection timeout in PyMongo?  
a) Retry the operation  
b) Ignore the error  
c) Use a larger batch size  

3. In mongosh, which feature is used to catch errors?  
a) try...except  
b) try...catch  
c) onError  

4. Why should you use specific exceptions before general ones in error handling?  
a) For performance  
b) For more precise and custom remediation  
c) For code brevity  

5. What does error code 121 indicate?  
a) Duplicate key  
b) Validation failure  
c) No such collection  

# References

- [MongoDB Error Handlers in Shell](https://www.mongodb.com/docs/mongodb-shell/snippets/error-handlers/)  
- [PyMongo Exception Reference](https://pymongo.readthedocs.io/en/stable/api/pymongo/errors.html)  
- [MongoDB Error Codes Official List](https://www.mongodb.com/docs/v7.0/reference/error-codes/)  
