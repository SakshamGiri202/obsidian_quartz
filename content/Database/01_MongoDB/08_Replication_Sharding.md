# MongoDB Replication & Sharding

## Replication

**Replication** maintains multiple copies of data across servers for high availability and fault tolerance.

### Replica Set
A **replica set** is a group of MongoDB nodes that maintain the same dataset.

#### Node Types

| Node | Role |
|------|------|
| **Primary** | Accepts all writes. Only one primary per replica set. |
| **Secondary** | Replicates data from primary. Can serve reads (optional). |
| **Arbiter** | Votes in elections but holds no data. Used for odd-numbered voting. |

#### How Replication Works
1. Primary receives all write operations
2. Primary records operations in **oplog** (operations log)
3. Secondaries copy oplog entries and apply them (asynchronous)
4. If primary fails, an **election** selects a new primary from secondaries
5. Majority of nodes (including arbiter) must participate in election

### Read Preference
```js
db.collection.find().readPref("primary")           // Default — read from primary
db.collection.find().readPref("secondary")          // Read from secondary
db.collection.find().readPref("primaryPreferred")   // Primary if available, else secondary
db.collection.find().readPref("secondaryPreferred") // Secondary if available, else primary
db.collection.find().readPref("nearest")            // Lowest latency node
```

### Write Concern
Controls acknowledgment level for writes:
```js
db.orders.insertOne(
  { item: "widget", qty: 100 },
  { writeConcern: { w: "majority", j: true, wtimeout: 5000 } }
)
```
- `w: 1` — Acknowledged by primary (default)
- `w: "majority"` — Acknowledged by majority of nodes
- `w: <number>` — Acknowledged by N nodes
- `j: true` — Written to journal
- `wtimeout` — Timeout in ms

### Read Concern
Controls data consistency:
```js
db.collection.find().readConcern("local")           // Default — current data (may be uncommitted)
db.collection.find().readConcern("majority")        // Only committed data
db.collection.find().readConcern("linearizable")    // Most current data (slower)
db.collection.find().readConcern("available")       // Fastest, for sharded clusters
```

### Election & Failover
- Triggered when primary is unreachable for >10 seconds
- Nodes vote using priority (higher = more likely to become primary)
- Uses **majority consensus** algorithm (Raft-based)
- Typically takes 10-30 seconds

### Oplog (Operations Log)
- A **capped collection** (`local.oplog.rs`) that records all write operations
- Size: 5% of free disk space by default (min 990MB, max 50GB)
- Secondaries replay from the oplog
- If a secondary falls behind the oplog window, it must be re-synced

---

## Sharding

**Sharding** horizontally partitions data across multiple servers to handle large datasets and high throughput.

### Sharded Cluster Components

```
┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│  Query      │  │  Query      │  │  Query      │
│  Router     │  │  Router     │  │  Router     │
│  (mongos)   │  │  (mongos)   │  │  (mongos)   │
└──────┬──────┘  └──────┬──────┘  └──────┬──────┘
       │                │                │
       └────────────────┼────────────────┘
                        │
       ┌────────────────┼────────────────┐
       │                │                │
┌──────┴──────┐  ┌──────┴──────┐  ┌──────┴──────┐
│  Config     │  │  Config     │  │  Config     │
│  Server     │  │  Server     │  │  Server     │
│  (Replica)  │  │  (Replica)  │  │  (Replica)  │
└─────────────┘  └─────────────┘  └─────────────┘

┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│  Shard 1    │  │  Shard 2    │  │  Shard N    │
│  (Replica)  │  │  (Replica)  │  │  (Replica)  │
└─────────────┘  └─────────────┘  └─────────────┘
```

| Component | Description |
|-----------|-------------|
| **Shard** | Stores a subset of data (each shard is a replica set) |
| **Config Server** | Stores metadata and cluster configuration (replica set) |
| **Query Router (mongos)** | Routes client requests to appropriate shards |

### Shard Key
A field or compound field that determines how data is distributed across shards.

**Choosing a Shard Key:**
- **High cardinality** — Many unique values (e.g., `userId`, `email`)
- **Low frequency** — Values distributed evenly
- **Monotonically increasing/decreasing** — Avoid (creates hotspots)

**Shard Key Strategies:**
- **Hashed Shard Key** — Even distribution, no range support
  ```js
  sh.shardCollection("myDb.collection", { userId: "hashed" })
  ```
- **Ranged Shard Key** — Supports range queries, may create hotspots
  ```js
  sh.shardCollection("myDb.collection", { timestamp: 1 })
  ```
- **Compound Shard Key** — Combines fields for better distribution
  ```js
  sh.shardCollection("myDb.collection", { country: 1, userId: 1 })
  ```

### Data Distribution
- **Chunks** — Contiguous ranges of shard key values
- MongoDB automatically splits chunks when they exceed **max chunk size** (64MB by default)
- **Balancer** — Migrates chunks across shards to maintain even distribution

### Operations
```js
// Enable sharding on database
sh.enableSharding("myDatabase")

// Shard a collection (must have index on shard key)
db.myCollection.createIndex({ userId: "hashed" })
sh.shardCollection("myDatabase.myCollection", { userId: "hashed" })

// Check cluster status
sh.status()

// List shards
db.adminCommand({ listShards: 1 })

// Move chunk manually
db.adminCommand({ moveChunk: "myDb.collection", find: { userId: 123 }, to: "shard2" })
```

### Targeted vs Broadcast Operations
- **Targeted Queries** — Include shard key → routed to specific shard (fast)
- **Broadcast Queries** — No shard key → sent to ALL shards (slow)
```js
db.collection.find({ userId: 123 })        // Targeted (uses shard key)
db.collection.find({ name: "Alice" })      // Broadcast (no shard key)
```

---

## Replication vs Sharding

| Aspect | Replication | Sharding |
|--------|-------------|----------|
| Purpose | High availability, fault tolerance | Horizontal scaling, large datasets |
| Data | Copies same data across servers | Splits data across servers |
| Components | Primary, Secondaries, Arbiter | Shards, Config Servers, mongos |
| Writes | Primary only | Routed by shard key |
| Reads | Can scale via secondaries | Routed by shard key |
| Failover | Automatic election | Shard-level replica set failover + mongos awareness |

---

## Production Best Practices

### Replication
- Deploy **odd number** of voting members (3 or more)
- Use **arbiter** only when resources are limited
- Keep secondaries **close to oplog window** (monitor replication lag)
- Use **majority write concern** for critical data
- Test failover scenarios regularly

### Sharding
- Choose shard key **carefully** — it cannot be changed after sharding
- Pre-shard collection if you know approximate data distribution
- Monitor **chunk distribution** and **balancer activity**
- Use **hashed shard key** for workload uniformity
- Deploy **at least 3 config servers** as a replica set
- Place **mongos** routers on application servers or behind a load balancer
