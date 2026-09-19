# MongoDB Introduction & NoSQL

## What is NoSQL?
**NoSQL** (Not Only SQL) is a category of database systems designed for large-scale, distributed data where relational models may not be optimal. They provide flexible schemas, horizontal scaling, and high availability.

### Why NoSQL?
- Handle large volumes of structured, semi-structured, and unstructured data
- Flexible schema evolution
- Horizontal scaling across commodity hardware
- High availability and fault tolerance
- Faster development cycles with agile data models

### Types of NoSQL Databases

| Type | Description | Examples |
|------|-------------|----------|
| **Document Stores** | Store data as JSON/BSON documents | MongoDB, CouchDB |
| **Key-Value Stores** | Simple key-value pairs | Redis, DynamoDB |
| **Column-Family Stores** | Store data in column families | Cassandra, HBase |
| **Graph Databases** | Store nodes and relationships | Neo4j, ArangoDB |

### CAP Theorem
A distributed database guarantees **two** of three:
- **Consistency** — All nodes see same data at same time
- **Availability** — Every request gets a response
- **Partition Tolerance** — System continues despite network partitions

MongoDB is **CP** by default (Consistent + Partition Tolerant).

---

## What is MongoDB?
**MongoDB** is an open-source, document-oriented NoSQL database designed for storing and managing large volumes of data efficiently using a flexible, JSON-like document model.

### Key Features
- **Document-Oriented** — Stores data as BSON (Binary JSON) documents
- **Schema-Flexible** — Documents in same collection can have different fields
- **Indexing** — Supports various index types for query optimization
- **Aggregation Pipeline** — Powerful data processing framework
- **Replication** — Replica sets for high availability
- **Sharding** — Horizontal scaling across clusters
- **Atomic Operations** — Document-level atomicity
- **Ad-hoc Queries** — Rich query language with operators
- **Drivers** — Official drivers for all major programming languages

### BSON vs JSON

| Aspect | JSON | BSON |
|--------|------|------|
| Encoding | UTF-8 String | Binary |
| Data Types | String, Number, Boolean, null, Array, Object | All JSON + Date, ObjectId, Binary, Regex, Int32, Int64, Decimal128 |
| Space Efficiency | Text-heavy | More compact binary |
| Traversal Speed | Slow (parse required) | Fast (length-prefixed) |

### When to Use MongoDB
✅ **Good Fit:**
- Rapid application development
- Hierarchical/nested data structures
- High write throughput
- Horizontal scaling needed
- Flexible/evolving schema
- Real-time analytics, IoT, content management

❌ **Not Ideal:**
- Complex multi-row transactions (though supported since v4.0)
- Highly normalized data with complex joins
- SQL-based reporting tools required

---

## RDBMS vs MongoDB

| Aspect | RDBMS | MongoDB |
|--------|-------|---------|
| Data Model | Tables (rows & columns) | Documents (JSON/BSON) |
| Schema | Fixed, predefined | Flexible, dynamic |
| Query Language | SQL | MQL (MongoDB Query Language) |
| Relationships | Foreign keys, JOINs | Embedded documents, references |
| ACID | Full ACID across tables | Document-level ACID, multi-doc transactions (v4.0+) |
| Scaling | Vertical (scale-up) | Horizontal (scale-out via sharding) |
| Indexing | B+ Tree, bitmap, etc. | B-Tree, text, geospatial, hashed, TTL |
| Primary Key | Auto-increment or UUID | `_id` field (ObjectId by default) |

### Terminology Mapping

| RDBMS | MongoDB |
|-------|---------|
| Database | Database |
| Table | Collection |
| Row | Document |
| Column | Field |
| Index | Index |
| JOIN | Embedded/References ($lookup aggregation) |
| Primary Key | `_id` field |
| Foreign Key | Manual reference / DBRef |

---

## Installation & Setup

### MongoDB Editions
- **MongoDB Community Server** — Free, open-source
- **MongoDB Enterprise Server** — Commercial with advanced security
- **MongoDB Atlas** — Fully managed cloud service

### Key Tools

| Tool | Description |
|------|-------------|
| **mongod** | MongoDB database server daemon |
| **mongos** | MongoDB shard router for sharded clusters |
| **mongosh** | MongoDB Shell (command-line interface) |
| **MongoDB Compass** | GUI for data visualization and queries |
| **MongoDB Atlas** | Fully managed cloud database service |
| **mongodump** | Utility for creating BSON backups |
| **mongorestore** | Utility for restoring BSON backups |
| **mongoimport** | Import JSON, CSV, TSV data |
| **mongoexport** | Export data to JSON, CSV |
