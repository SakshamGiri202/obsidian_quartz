# Advanced Database Topics

## Data Warehousing

A **Data Warehouse** is a large, centralized repository of data collected from multiple sources, optimized for analysis and reporting rather than transaction processing.

### Characteristics
- **Subject-Oriented** — Organized around key subjects (customers, sales, products)
- **Integrated** — Consistent naming, formatting, and encoding
- **Time-Variant** — Historical data maintained for years
- **Non-Volatile** — Data is read-only once stored

### OLTP vs OLAP

| Aspect     | OLTP                   | OLAP                          |
| ---------- | ---------------------- | ----------------------------- |
| Purpose    | Transaction processing | Analysis and reporting        |
| Users      | Customers, clerks      | Analysts, managers            |
| Queries    | Simple, frequent       | Complex, less frequent        |
| Data       | Current, detailed      | Historical, summarized        |
| Operations | Read/Write             | Mostly Read                   |
| Design     | Normalized             | Denormalized (star/snowflake) |

### Data Warehouse Architecture
1. **Bottom Tier** — Database server (data warehouse)
2. **Middle Tier** — OLAP server
3. **Top Tier** — Frontend tools (reporting, analysis)

### Schemas
- **Star Schema** — One fact table connected to multiple dimension tables
- **Snowflake Schema** — Dimension tables are normalized (further sub-dimensions)
- **Fact Constellation (Galaxy)** — Multiple fact tables sharing dimension tables

---

## Data Mining

**Data Mining** is the process of discovering patterns, correlations, and knowledge from large datasets.

### Key Techniques
| Technique | Description |
|-----------|-------------|
| **Classification** | Assign items to predefined categories (e.g., spam detection) |
| **Clustering** | Group similar items without predefined categories |
| **Association Rule Mining** | Find relationships (e.g., market basket analysis: "people who bought X also bought Y") |
| **Regression** | Predict continuous values (e.g., sales forecasting) |
| **Anomaly Detection** | Identify unusual patterns (e.g., fraud detection) |
| **Sequential Pattern Mining** | Find patterns in sequence data |

### Applications
- Market basket analysis
- Fraud detection
- Customer segmentation
- Predictive analytics
- Recommendation systems

---

## NoSQL Databases

**NoSQL** (Not Only SQL) databases are designed for large-scale, distributed data where relational models may not be optimal.

### Categories

| Type | Description | Examples |
|------|-------------|----------|
| **Document Stores** | Store data as JSON/BSON documents | MongoDB, CouchDB |
| **Key-Value Stores** | Simple key-value pairs | Redis, DynamoDB |
| **Column-Family Stores** | Store data in column families | Cassandra, HBase |
| **Graph Databases** | Store nodes and relationships | Neo4j, ArangoDB |

### CAP Theorem
A distributed database can only guarantee **two** of the following three:

| Property | Description |
|----------|-------------|
| **Consistency (C)** | All nodes see the same data at the same time |
| **Availability (A)** | Every request receives a response (success or failure) |
| **Partition Tolerance (P)** | System continues operating despite network partitions |

**Common Combinations:**
- **CP** — Consistent and Partition Tolerant (HBase, MongoDB)
- **AP** — Available and Partition Tolerant (Cassandra, CouchDB)
- **CA** — Consistent and Available (traditional RDBMS — not possible in distributed systems)

### BASE Properties (NoSQL)
- **Basically Available** — System guarantees availability
- **Soft State** — State may change over time
- **Eventual Consistency** — System becomes consistent over time

---

## Distributed Databases

A **Distributed Database** is a collection of multiple logically interrelated databases distributed across a computer network.

### Advantages
- Reliability and availability
- Faster data access for local users
- Scalability
- Modular growth

### Challenges
- Data fragmentation and replication
- Distributed query processing
- Concurrency control across sites
- Transaction management (2-phase commit)
- Network communication overhead

### Data Distribution Strategies
- **Fragmentation** — Horizontal (rows), Vertical (columns), Mixed
- **Replication** — Full (all sites), Partial (some sites)
- **Hybrid** — Combination of fragmentation and replication

---

## Query Optimization

**Query Optimization** selects the most efficient execution plan for a query.

### Steps
1. **Parsing** — Check syntax and semantics
2. **Query Rewriting** — Convert to equivalent but more efficient form
3. **Plan Generation** — Generate alternative execution plans
4. **Cost Estimation** — Estimate CPU, I/O, network costs
5. **Plan Selection** — Choose the plan with lowest estimated cost

### Optimization Techniques
- **Index Selection** — Use existing indexes for faster access
- **Join Ordering** — Reorder joins to reduce intermediate result sizes
- **Predicate Pushdown** — Apply filters as early as possible
- **Projection Pushdown** — Select only needed columns
- **Materialization** — Store intermediate results temporarily
- **Pipelining** — Stream results between operations without storing

---

## Object-Relational Mapping (ORM)

**ORM** is a technique that maps database tables to objects in programming languages.

### Popular ORMs
| Language | ORM |
|----------|-----|
| Python | SQLAlchemy, Django ORM |
| Java | Hibernate, JPA |
| JavaScript/Node | Sequelize, TypeORM, Prisma |
| Ruby | ActiveRecord |
| PHP | Doctrine, Eloquent |
| C# | Entity Framework |

### Advantages
- Reduces boilerplate SQL code
- Object-oriented paradigm alignment
- Database abstraction (switch DBMS easily)
- Built-in caching, lazy loading, eager loading

### Disadvantages
- Performance overhead (generated SQL may not be optimal)
- Learning curve for complex queries
- N+1 query problem
- Limited support for advanced database features

---

## Database Performance Tuning

### Key Metrics
- Query response time
- Throughput (transactions per second)
- CPU utilization
- I/O wait time
- Cache hit ratio
- Lock contention

### Tuning Strategies
1. **Schema Optimization** — Proper normalization/denormalization
2. **Index Optimization** — Create/drop/modify indexes based on query patterns
3. **Query Optimization** — Use EXPLAIN plans, rewrite slow queries
4. **Configuration Tuning** — Buffer pool size, cache settings, connection pool
5. **Hardware Optimization** — Faster disks (SSD), more memory, better CPU
6. **Caching** — Implement application-level caching (Redis, Memcached)
7. **Partitioning** — Split large tables into smaller partitions
8. **Connection Pooling** — Reuse database connections
