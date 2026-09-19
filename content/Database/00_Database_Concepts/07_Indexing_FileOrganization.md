# Indexing & File Organization

## File Organization

**File Organization** defines how data records are mapped to disk blocks for efficient access.

### Types of File Organization

| Type | Description | Pros | Cons |
|------|-------------|------|------|
| **Sequential** | Records stored in sorted order based on a key | Fast for sequential access, range queries | Slow inserts/deletes |
| **Heap** | Records inserted at end of file, no ordering | Fast inserts for bulk loading | Slow search (full scan) |
| **Hash** | Hash function maps key to bucket | Fast exact-match queries | Poor range queries |
| **Clustered** | Related records stored together | Fast for joins, range queries on cluster key | Only one way to cluster |
| **ISAM** | Indexed Sequential Access Method | Good for both sequential and random | Static structure, degrades over time |
| **B+ Tree** | Balanced tree structure | Consistent performance for all operations | Overhead for small datasets |

### Heap File Organization
- Records inserted at end of file
- No sorting or ordering
- Searching requires traversing from beginning
- Fast for bulk loading, slow for selective queries

### Sequential File Organization
- Records stored in sorted order by a key field
- Uses **dense index** (one index entry per record) or **sparse index** (one per block)
- Search cost: O(log₂n) for index + 1 for block access

### Hash File Organization
- Hash function computes bucket address from key
- **Static Hashing** — Fixed number of buckets
- **Dynamic Hashing** — Buckets grow/shrink as needed (extendable hashing)
- Good for equality searches, poor for range queries

### Clustered File Organization
- Records with same cluster key value stored in same/d adjacent blocks
- Reduces I/O for operations on related records
- Improves join performance

---

## Indexing

**Indexing** is a data structure technique to speed up data retrieval by minimizing disk accesses. It stores copies of selected column values with pointers to actual data rows.

### Indexing Attributes
- **Access Types** — Value-based search, range access, etc.
- **Access Time** — Time to find a data element
- **Insertion Time** — Time to insert new data including index update
- **Deletion Time** — Time to delete including index update
- **Space Overhead** — Additional space required by the index

---

## Types of Indexing

### 1. Primary Indexing
- Created on the **primary key** of a data file
- Data records are physically stored in sorted order by the primary key
- Each index entry corresponds to a **block** (not each record)
- Contains primary key value + pointer to first record of the block

### 2. Clustered Indexing
- Stores related records together in the same/adjacent blocks
- Data is physically ordered by the indexed column
- Can be created on non-primary key columns to group similar records
- If indexed column is not unique, composite keys can be used

### 3. Non-Clustered (Secondary) Indexing
- Data is NOT physically ordered by the index
- Index contains ordered references (pointers) to data locations
- Only dense indexing is possible (sparse not possible since data is unordered)
- Requires extra step to follow pointer to data
- Like the index of a book — ordered list with page references

### 4. Multilevel Indexing
- When the index itself becomes too large to fit in memory
- Outer level index points to inner level index blocks
- Inner level index points to data blocks
- Reduces memory overhead and speeds up query execution

### Comparison: Clustered vs Non-Clustered

| Aspect | Clustered | Non-Clustered |
|--------|-----------|---------------|
| Data Ordering | Physical order matches index | Physical order independent of index |
| Number per Table | One (data can be sorted only one way) | Multiple |
| Speed | Faster for range queries, retrieval | Slightly slower (extra pointer lookup) |
| Space | No extra space for data copy | Extra space for index structure |
| Index Type | Can be dense or sparse | Always dense |

---

## B-Tree & B+ Tree Indexing

### B-Tree
- Self-balancing tree data structure
- Maintains sorted data for efficient insert/delete/search
- All nodes (internal + leaf) contain data pointers
- Each node can have multiple children
- Depth is logarithmic, ensuring consistent performance

**Properties:**
- All leaves at same depth
- Each node has between ⌈m/2⌉ and m children (m = order of tree)
- Root has between 2 and m children (except leaf root)

### B+ Tree
- Variant of B-Tree used in most database systems
- **Internal nodes** — Only contain keys (routers)
- **Leaf nodes** — Contain keys + pointers to data records
- Leaf nodes are linked (sequential access)
- More space efficient than B-Tree for most workloads

| Aspect | B-Tree | B+ Tree |
|--------|--------|---------|
| Data Pointers | All nodes | Only leaf nodes |
| Internal Nodes | Keys + data pointers | Only keys |
| Leaf Nodes Linked | No | Yes |
| Range Queries | Slow (traverse up/down) | Fast (follow leaf chain) |
| Space Utilization | Less efficient | More space for keys |
| Used In | Oracle (some cases) | MySQL InnoDB, PostgreSQL, SQL Server |

---

## Other Indexing Techniques

### Bitmap Indexing
- Uses bit arrays (bitmaps) for each distinct value
- Efficient for columns with low cardinality (few distinct values)
- Fast for Boolean operations (AND, OR, NOT on bitmaps)
- Used in data warehousing / OLAP

### Inverted Index
- Maps content (words, terms) to their locations in documents
- Used in full-text search engines
- Each term has a list of document IDs containing it
- Supports fast full-text searches

### Hash Indexing
- Uses hash function to map keys directly to bucket addresses
- O(1) lookup for equality conditions
- Not suitable for range queries (no ordering)
- Used in key-value stores

---

## Indexing Strategies & Best Practices

### When to Index
- Columns frequently used in WHERE, JOIN, ORDER BY
- Columns with high selectivity (many unique values)
- Foreign key columns (improves join performance)
- Large tables where full scan is expensive

### When NOT to Index
- Small tables (full scan is fast enough)
- Columns rarely used in queries
- Columns with low cardinality (e.g., boolean)
- Tables with heavy write operations (index maintenance cost)

### Index Maintenance
- **Rebuild** — Reorganizes and rebuilds index (removes fragmentation)
- **Reorganize** — Defragments leaf level
- **Update Statistics** — Helps query optimizer choose best execution plan
