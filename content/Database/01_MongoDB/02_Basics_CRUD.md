# MongoDB Basics & CRUD Operations

## Core Concepts

### Database
A container for collections. Each database gets its own set of files on the filesystem.
```js
use myDatabase       // Switch/create database
db                   // Show current database
show dbs             // List all databases
db.dropDatabase()    // Delete current database
```

### Collection
A group of documents (analogous to a table in RDBMS). Collections have **dynamic schemas** — documents in the same collection can have different fields.
```js
db.createCollection("users")   // Create collection explicitly
show collections               // List all collections
db.users.drop()                // Drop collection
```

Collections are created **implicitly** on first document insert.

### Document
A set of key-value pairs stored as BSON (Binary JSON). The fundamental unit of data in MongoDB.
```js
{
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "name": "Alice",
  "age": 30,
  "email": "alice@example.com",
  "address": {
    "city": "New York",
    "zip": "10001"
  },
  "hobbies": ["reading", "coding"]
}
```

### `_id` Field
- Every document must have an `_id` field as the primary key
- Auto-generated as `ObjectId` if not provided
- `ObjectId` is 12 bytes: 4-byte timestamp + 5-byte random + 3-byte increment
- Can be any unique value (UUID, integer, string)

### ObjectId
```js
ObjectId()                          // Generate new ObjectId
ObjectId("507f1f77bcf86cd799439011").getTimestamp()  // Get creation timestamp
```

### Cursor
A pointer to the result set of a query. Cursors are iterated lazily as documents are requested.
```js
const cursor = db.users.find()
cursor.hasNext()        // Check if more documents exist
cursor.next()           // Get next document
cursor.toArray()        // Get all documents as array
cursor.forEach(doc => printjson(doc))
```

---

## CRUD Operations

### Create (Insert)

#### Single Document
```js
db.users.insertOne(
  { name: "Alice", age: 30, email: "alice@example.com" }
)
// Returns: { acknowledged: true, insertedId: ObjectId("...") }
```

#### Multiple Documents
```js
db.users.insertMany([
  { name: "Bob", age: 25 },
  { name: "Charlie", age: 35 },
  { name: "Diana", age: 28 }
])
// Returns: { acknowledged: true, insertedIds: { "0": ObjectId(...), "1": ObjectId(...), ... } }
```

#### Insert Options
- `ordered: true` (default) — Stop on first error
- `ordered: false` — Continue inserting despite errors

#### Bulk Write
```js
db.users.bulkWrite([
  { insertOne: { document: { name: "Eve" } } },
  { updateOne: { filter: { name: "Bob" }, update: { $set: { age: 26 } } } },
  { deleteOne: { filter: { name: "Charlie" } } }
])
```

### Read (Find)

#### Find All
```js
db.users.find()                    // All documents
db.users.find().pretty()           // Formatted output
```

#### Find with Filter
```js
db.users.find({ age: 30 })                                   // Exact match
db.users.find({ age: { $gt: 25 } })                          // Greater than
db.users.find({ name: "Alice", age: 30 })                    // AND (implicit)
db.users.find({ $or: [{ age: 25 }, { age: 35 }] })           // OR
db.users.find({ name: { $in: ["Alice", "Bob"] } })           // IN
```

#### Projection
Specify which fields to include/exclude:
```js
db.users.find({}, { name: 1, email: 1, _id: 0 })   // Only name and email
```

#### Find One
```js
db.users.findOne({ name: "Alice" })   // Returns single document (or null)
```

#### Query Methods
```js
db.users.find().sort({ age: 1 })       // Ascending (1) / Descending (-1)
db.users.find().limit(5)               // Limit results
db.users.find().skip(10)               // Skip N documents
db.users.find().count()                // Count results (deprecated)
db.users.countDocuments({ age: { $gt: 25 } })  // Accurate count with filter
db.users.estimatedDocumentCount()      // Estimated count (fast)
db.users.distinct("age")               // Get unique values for field
```

### Update

#### Update One
```js
db.users.updateOne(
  { name: "Alice" },               // Filter
  { $set: { age: 31 } }            // Update operation
)
```

#### Update Many
```js
db.users.updateMany(
  { age: { $lt: 30 } },
  { $set: { status: "young" } }
)
```

#### Replace One
```js
db.users.replaceOne(
  { name: "Alice" },
  { name: "Alice", age: 32, email: "alice_new@example.com" }
)
```

#### Upsert
Creates a new document if no match is found:
```js
db.users.updateOne(
  { name: "Frank" },
  { $set: { age: 40 } },
  { upsert: true }
)
```

#### findAndModify variants
```js
db.users.findOneAndUpdate(filter, update, options)   // Find and update, return document
db.users.findOneAndDelete(filter, options)            // Find and delete, return document
db.users.findOneAndReplace(filter, replacement, options)
```

### Delete

#### Delete One
```js
db.users.deleteOne({ name: "Charlie" })
// Returns: { acknowledged: true, deletedCount: 1 }
```

#### Delete Many
```js
db.users.deleteMany({ age: { $lt: 20 } })      // Delete all matching
db.users.deleteMany({})                          // Delete ALL documents!
```

#### Remove (Deprecated)
```js
db.users.remove({ name: "Charlie" })   // Prefer deleteOne/deleteMany
```

---

## Capped Collections
Fixed-size collections that maintain insertion order and automatically remove oldest documents when size limit is reached.
```js
db.createCollection("logs", {
  capped: true,
  size: 100000,          // Size in bytes
  max: 5000              // Maximum number of documents
})
```
- Good for logs, cache data, auto-expiring data
- Insertion order is preserved
- Cannot delete documents individually
- Tailable cursors for "watch" behavior
