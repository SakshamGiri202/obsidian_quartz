# MongoDB Advanced Topics

## Transactions

MongoDB supports **multi-document ACID transactions** since version 4.0 (replica sets) and 4.2 (sharded clusters).

### Transaction API
```js
// Start a session
const session = db.getMongo().startSession()

try {
  session.startTransaction({
    readConcern: { level: "snapshot" },
    writeConcern: { w: "majority" }
  })

  const accounts = session.getDatabase("bank").getCollection("accounts")
  const transfers = session.getDatabase("bank").getCollection("transfers")

  accounts.updateOne(
    { accountId: "A1" },
    { $inc: { balance: -100 } },
    { session }
  )

  accounts.updateOne(
    { accountId: "A2" },
    { $inc: { balance: 100 } },
    { session }
  )

  transfers.insertOne(
    { from: "A1", to: "A2", amount: 100, date: new Date() },
    { session }
  )

  session.commitTransaction()
} catch (error) {
  session.abortTransaction()
} finally {
  session.endSession()
}
```

### Transaction Options

| Option | Values | Description |
|--------|--------|-------------|
| `readConcern` | `local`, `majority`, `snapshot`, `linearizable` | Isolation level |
| `writeConcern` | `1`, `majority`, `<number>` | Acknowledgment level |
| `readPreference` | `primary`, `secondary`, etc. | Read routing |

### Limitations
- Max transaction runtime: **default 60 seconds** (configurable)
- Max **lock limit**: default 100 locks
- **Operations**: CRUD only (no DDL operations like `createCollection`)
- **Collection must exist** before transaction starts
- **16MB** total oplog entry per transaction (sharded)

### When to Use Transactions
✅ Banking transfers, inventory management, order processing
❌ Single-document operations (atomic by default in MongoDB)

---

## Change Streams

**Change streams** allow applications to watch real-time changes on collections, databases, or deployments.

```js
// Watch all changes in a collection
const changeStream = db.collection("orders").watch()

// Watch with pipeline — filter specific changes
const changeStream = db.collection("orders").watch([
  { $match: { "fullDocument.status": "shipped" } }
])

// Listen for changes
while (!changeStream.isClosed()) {
  const change = changeStream.tryNext()
  if (change) {
    printjson(change)
  }
}
```

### Change Event Document
```js
{
  _id: { _data: "8261..." },
  operationType: "insert",     // insert, update, replace, delete, drop, rename, invalidate
  clusterTime: Timestamp(1, 1),
  ns: { db: "myDb", coll: "orders" },
  documentKey: { _id: ObjectId("...") },
  fullDocument: { ... },       // For insert, replace, update (with fullDocument: "required")
  updateDescription: {         // For update operations
    updatedFields: { status: "shipped" },
    removedFields: []
  }
}
```

### Resume Tokens
Change streams are **resumable** using resume tokens:
```js
const resumeToken = changeStream.resumeToken
// Store token, reuse later
const newStream = db.collection("orders").watch([], { resumeAfter: resumeToken })
```

### Use Cases
- Real-time notifications
- Cache invalidation
- Replicating data to other systems
- Event-driven architectures
- Audit logging

---

## Text Search

### Create Text Index
```js
db.articles.createIndex({ title: "text", body: "text" })
```

### Search Operations
```js
// Basic search
db.articles.find({ $text: { $search: "mongodb database" } })

// Phrase search
db.articles.find({ $text: { $search: "\"high performance\"" } })

// Exclusion
db.articles.find({ $text: { $search: "database -sql" } })

// With language
db.articles.find({ $text: { $search: "base de datos", $language: "es" } })

// With relevance scoring
db.articles.find(
  { $text: { $search: "mongodb" } },
  { score: { $meta: "textScore" } }
).sort({ score: { $meta: "textScore" } })
```

### Weighted Text Indexes
```js
db.articles.createIndex(
  { title: "text", body: "text" },
  { weights: { title: 10, body: 3 } }
)
// Title matches are weighted ~3x more than body matches
```

### Atlas Search
MongoDB Atlas provides **Lucene-based** full-text search with:
- Analyzers, tokenizers, and filters
- Fuzzy matching
- Autocomplete
- Synonyms
- Faceted search
- More advanced than MongoDB's built-in text search

---

## Capped Collections

Fixed-size collections with automatic eviction of oldest documents:
```js
db.createCollection("logs", {
  capped: true,
  size: 100000,        // Size in bytes (required)
  max: 5000            // Maximum number of documents (optional)
})

// Check if collection is capped
db.logs.isCapped()

// Convert existing collection to capped
db.runCommand({ convertToCapped: "logs", size: 100000 })
```

### Properties
- **Insertion order preserved**
- No individual document deletion
- Tailable cursors for watching new documents
- Good for: logs, auto-expiring data, high-throughput event storage

### Tailable Cursor
```js
const cursor = db.logs.find().addOption(DBQuery.Option.tailable)
while (cursor.hasNext()) {
  printjson(cursor.next())
}
// Waits for new documents (like tail -f)
```

---

## Time Series Collections

MongoDB 5.0+ has **native time series support**:
```js
db.createCollection("weather", {
  timeseries: {
    timeField: "timestamp",
    metaField: "location",
    granularity: "hours"      // seconds, minutes, hours
  }
})

// Insert time-series data
db.weather.insertOne({
  timestamp: ISODate("2024-01-01T00:00:00Z"),
  location: "NYC",
  temperature: 72.5,
  humidity: 0.45
})
```

**Optimizations:**
- Automatic organization for time-based queries
- Compressed storage
- Limited updates (only metaField and future timeFields)

---

## MongoDB Drivers & ODM

### Official Drivers
MongoDB provides official drivers for all major languages:
- **Node.js** — `mongodb` (native) or `mongoose` (ODM)
- **Python** — `pymongo` (native) or `mongoengine` (ODM)
- **Java** — `mongodb-driver-sync` or `Spring Data MongoDB`
- **C#/.NET** — `MongoDB.Driver`
- **Go** — `go.mongodb.org/mongo-driver`
- **Ruby** — `mongo` gem
- **PHP** — `mongodb` extension + `mongodb/mongodb` library

### Mongoose (Node.js ODM)
```js
const mongoose = require('mongoose')
mongoose.connect('mongodb://localhost:27017/myDatabase')

// Schema definition
const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, unique: true },
  age: { type: Number, min: 0 },
  createdAt: { type: Date, default: Date.now }
})

// Model
const User = mongoose.model('User', userSchema)

// CRUD
const user = await User.create({ name: "Alice", email: "alice@example.com" })
const users = await User.find({ age: { $gte: 18 } }).sort({ name: 1 })
```

---

## Aggregation Pipeline Update Operators (MongoDB 4.2+)

Updates can use aggregation pipeline expressions:
```js
// Before v4.2 (cannot reference other fields)
db.users.updateMany({}, { $set: { age: 30 } })  // Sets ALL to 30

// v4.2+ (computed updates)
db.users.updateMany(
  { status: "active" },
  [{ $set: { age: { $add: ["$age", 1] }, lastUpdated: "$$NOW" } }]
)

// Conditional updates
db.orders.updateMany(
  { status: "pending" },
  [{
    $set: {
      discountedPrice: {
        $cond: {
          if: { $gte: ["$total", 100] },
          then: { $multiply: ["$total", 0.9] },
          else: "$total"
        }
      }
    }
  }]
)
```

---

## Serverless & Atlas

### MongoDB Atlas Serverless (Preview)
- Auto-scaling from 0 to high throughput
- Pay-per-use pricing (no idle cost)
- Automatic backups and patching
- Good for: variable workloads, prototypes, low-traffic apps

### Atlas Data Federation
Query data across multiple sources:
- S3 buckets (Parquet, JSON, CSV, Avro)
- Atlas clusters
- HTTP sources
- Online archives

### Atlas Online Archive
Automatically archive old data to S3:
```js
// Define archiving rules in Atlas UI or API
// Data older than N days moves to low-cost S3 storage
// Queries automatically include archived data
```

---

## Performance Monitoring

### Profiler
```js
// Enable profiling
db.setProfilingLevel(1, { slowms: 100 })    // Log slow queries (>100ms)
db.setProfilingLevel(2)                        // Log all queries
db.setProfilingLevel(0)                        // Disable

// View slow queries
db.system.profile.find().sort({ ts: -1 }).limit(10).pretty()
```

### Monitoring Commands
```js
db.serverStatus()            // Server health and stats
db.stats()                   // Database statistics
db.collection.stats()        // Collection statistics
db.currentOp()               // Currently running operations
db.killOp(opId)              // Kill a running operation
```

### Atlas Monitoring
- **Real-time Performance Panel** — Live query activity
- **Metrics** — Connections, operations, memory, disk, network
- **Alerts** — CPU, memory, connection threshold alerts
- **Advisor** — Index suggestions and query optimization tips

---

## Summary of Key Strengths

| Feature | Status |
|---------|--------|
| Flexible Schema | ✅ Native |
| Horizontal Scaling | ✅ Sharding |
| High Availability | ✅ Replica Sets |
| ACID Transactions | ✅ v4.0+ |
| Rich Queries | ✅ MQL |
| Aggregation | ✅ Pipeline |
| Full-Text Search | ✅ Built-in + Atlas Search |
| Geospatial | ✅ 2dsphere, GeoJSON |
| Time Series | ✅ v5.0+ |
| Change Streams | ✅ Real-time CDC |
| Security | ✅ SCRAM, x.509, LDAP, Kerberos, FLE |
| Multi-Cloud | ✅ Atlas |
