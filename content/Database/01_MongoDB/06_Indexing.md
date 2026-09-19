# MongoDB Indexing

## What is Indexing?
An **index** is a special data structure that stores a subset of collection data in an ordered format, enabling MongoDB to quickly locate documents without scanning the entire collection.

Without an index, MongoDB must perform a **collection scan** — checking every document — which becomes slow as data grows.

### How Indexes Work
```
Collection (no index): Scan ALL 1M documents → Find match
Collection (with index): Lookup in B-Tree → Directly locate documents
```

---

## Creating & Managing Indexes

### Create Index
```js
db.users.createIndex({ name: 1 })            // Ascending
db.users.createIndex({ age: -1 })             // Descending

// With options
db.users.createIndex({ email: 1 }, { unique: true })
db.users.createIndex({ createdAt: 1 }, { expireAfterSeconds: 86400 })  // TTL
db.users.createIndex({ name: 1 }, { sparse: true })                    // Only index docs with field
db.users.createIndex({ name: 1 }, { background: true })                // Build in background
db.users.createIndex({ name: 1, age: -1 }, { name: "name_age_idx" })  // Named index
```

### View Indexes
```js
db.users.getIndexes()           // All indexes in collection
```

### Drop Indexes
```js
db.users.dropIndex({ name: 1 })            // Drop specific
db.users.dropIndexes()                     // Drop all (except _id)
db.users.dropIndexes(["idx1", "idx2"])     // Drop multiple
```

### Index Usage Information
```js
db.users.find({ name: "Alice" }).explain("executionStats")
// Shows: winningPlan, totalDocsExamined, executionTimeMillis, etc.
```

---

## Types of Indexes

### 1. Single Field Index
```js
db.users.createIndex({ name: 1 })
```
- Simple index on one field
- Supports both ascending (1) and descending (-1)

### 2. Compound Index
```js
db.users.createIndex({ name: 1, age: -1, status: 1 })
```
- Index on multiple fields
- Order of fields matters for query coverage
- Supports **prefix matching** — queries using prefix of indexed fields

**ESR Rule** (Equality → Sort → Range):
```js
// Query: db.users.find({ status: "active" }).sort({ age: -1 }).limit(10)
// Index: db.users.createIndex({ status: 1, age: -1 })
```
Place equality-match fields first, then sort fields, then range fields.

### 3. Multikey Index
```js
db.users.createIndex({ tags: 1 })       // Array field
```
- Automatically created when indexing an array field
- Creates index entries for each array element
- A collection can have at most one array field per index

### 4. Text Index
```js
db.articles.createIndex({ content: "text", title: "text" })   // Multiple fields
db.articles.createIndex({ content: "text" }, { weights: { title: 10, content: 5 } })  // Weighted
db.articles.createIndex({ "$**": "text" })                      // All string fields
```

**Text Search:**
```js
db.articles.find({ $text: { $search: "mongodb tutorial" } })
db.articles.find({ $text: { $search: "\"exact phrase\"" } })   // Phrase search
db.articles.find({ $text: { $search: "mongodb -sql" } })       // Exclude "sql"

// With relevance score
db.articles.find(
  { $text: { $search: "mongodb" } },
  { score: { $meta: "textScore" } }
).sort({ score: { $meta: "textScore" } })
```

- A collection can have **at most one text index**
- Requires creating a text index first

### 5. Hashed Index
```js
db.users.createIndex({ userId: "hashed" })
```
- Index entries are hashes of the field value
- Good for **shard keys** (even distribution)
- Only supports **equality queries** (no range)
- Cannot be compound (except unique constraint)

### 6. TTL (Time-To-Live) Index
```js
db.sessions.createIndex({ createdAt: 1 }, { expireAfterSeconds: 3600 })
```
- Automatically removes documents after specified time
- Only works on date fields
- Background job runs every 60 seconds
- Cannot be compound

### 7. Geospatial Index
```js
// 2dsphere for GeoJSON objects
db.places.createIndex({ location: "2dsphere" })

// 2d for legacy coordinate pairs
db.places.createIndex({ location: "2d" })
```

```js
db.places.find({
  location: {
    $near: { $geometry: { type: "Point", coordinates: [-73.97, 40.77] }, $maxDistance: 1000 }
  }
})
```

### 8. Unique Index
```js
db.users.createIndex({ email: 1 }, { unique: true })
```
- Ensures all values in indexed field are unique
- `_id` index is unique by default
- Cannot be used on hashed indexes

### 9. Partial Index
```js
db.users.createIndex(
  { name: 1 },
  { partialFilterExpression: { status: "active" } }
)
```
- Only indexes documents matching the filter
- Reduces index size and overhead

### 10. Sparse Index
```js
db.users.createIndex({ email: 1 }, { sparse: true })
```
- Only indexes documents that contain the indexed field
- Useful when field exists only in some documents

### 11. Hidden Index
```js
db.users.createIndex({ name: 1 }, { hidden: true })
```
- Index exists but is not used by the query planner
- Used for testing impact of dropping an index

### 12. Wildcard Index
```js
db.users.createIndex({ "$**": 1 })                          // All fields
db.users.createIndex({ "metadata.$**": 1 })                 // Specific path
```
- Indexes all fields (or fields under a path)
- Useful for schemas with unpredictable field names

---

## Query Optimization

### explain()
```js
db.users.find({ name: "Alice" }).explain("queryPlanner")      // Default
db.users.find({ name: "Alice" }).explain("executionStats")    // With execution stats
db.users.find({ name: "Alice" }).explain("allPlansExecution") // All plans
```

Key metrics in `executionStats`:
- `totalDocsExamined` — Documents scanned
- `totalKeysExamined` — Index entries scanned
- `executionTimeMillis` — Query duration
- `nReturned` — Documents returned
- `stage` — `COLLSCAN` (bad), `IXSCAN` (good), `FETCH`

### Query Selectivity
- **High selectivity** (many unique values) → Index is effective
- **Low selectivity** (few unique values) → Index may not help much

### Covered Query
A query where **all required fields are in the index** — MongoDB never needs to fetch documents.
```js
db.users.createIndex({ name: 1, email: 1 })
db.users.find({ name: "Alice" }, { name: 1, email: 1, _id: 0 })
// Stage: IXSCAN only (no FETCH)
```

---

## Indexing Best Practices

### Do ✅
- Index fields used in `find()`, `sort()`, and `$lookup`
- Use **ESR rule** for compound indexes
- Create indexes to support your most frequent queries
- Use `explain()` to verify index usage
- Monitor slow queries with `db.setProfilingLevel(1)`
- Consider **partial** indexes for filtered queries
- Drop unused indexes (use `$indexStats`)

### Don't ❌
- Over-index (each index adds write overhead)
- Use `multikey` indexes unnecessarily (one per collection max)
- Index every field blindly
- Create indexes without testing query patterns

### Index Stats
```js
db.users.aggregate([ { $indexStats: {} } ])
// Shows: name, accesses, ops, since
```
