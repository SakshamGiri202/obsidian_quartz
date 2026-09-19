# MongoDB Data Modeling

## Overview
**Data modeling** in MongoDB focuses on structuring documents and collections to match application access patterns, optimize query performance, and support scalability.

### Key Considerations
- **Application access patterns** — How will data be read and written?
- **Data relationships** — One-to-one, one-to-many, many-to-many
- **Query patterns** — What queries will be most frequent?
- **Growth expectations** — How will data scale over time?
- **Atomicity requirements** — What operations must be atomic?

---

## Data Model Approaches

### 1. Embedded Data Model
Store related data within a single document as **sub-documents** or **arrays**.

```js
{
  _id: ObjectId(),
  name: "Alice",
  address: {
    street: "123 Main St",
    city: "New York",
    zip: "10001"
  },
  orders: [
    { productId: 1, quantity: 2, price: 29.99 },
    { productId: 3, quantity: 1, price: 49.99 }
  ]
}
```

**When to Embed:**
- **Contains** relationships (document contains sub-items)
- One-to-one or one-to-few relationships
- Data accessed together (reduces reads)
- Data updated together (atomic operations)

**Pros:**
- Faster reads (single query fetches all related data)
- Atomic updates on the entire document
- No joins needed

**Cons:**
- 16MB document size limit
- Data duplication (if same data embedded in multiple documents)
- Difficult to access sub-documents independently

### 2. Normalized (Referenced) Data Model
Store related data in **separate collections** linked by references (IDs).

```js
// Users collection
{ _id: ObjectId("user1"), name: "Alice" }

// Orders collection
{ _id: ObjectId("order1"), userId: "user1", items: ["prod1", "prod2"] }

// Products collection
{ _id: ObjectId("prod1"), name: "Widget", price: 29.99 }
```

**When to Reference:**
- **Has** relationships (entity has many related entities)
- Many-to-many relationships
- Large, independent data sets
- Data accessed independently

**Pros:**
- No data duplication
- Smaller documents
- Individual entities can be managed independently

**Cons:**
- Slower reads (multiple queries or `$lookup`)
- No atomicity across collections
- More complex query logic

---

## Relationship Patterns

### One-to-One (1:1)
```js
// Embedded (preferred)
{ _id: 1, name: "Alice", profile: { avatar: "a.png", bio: "Hi" } }

// Referenced
{ _id: 1, name: "Alice", profileId: 101 }
```
**Recommendation:** Embed (data is always accessed together)

### One-to-Many (1:N)
```js
// One-to-few (embed)
{ _id: 1, name: "Alice", addresses: [{ type: "home", city: "NYC" }, { type: "work", city: "LA" }] }

// One-to-many (reference)
// User: { _id: 1, name: "Alice" }
// Orders: { _id: 101, userId: 1, total: 50 }  // references user

// One-to-squillions (reference from parent)
// User: { _id: 1, name: "Alice" }
// Logs: { _id: 10001, userId: 1, action: "login" }
```
- **One-to-few:** Embed (a few addresses per user)
- **One-to-many:** Reference from child (orders → user)
- **One-to-squillions:** Reference from child, index the foreign key

### Many-to-Many (N:M)
```js
// Students: { _id: 1, name: "Alice", courseIds: [101, 102] }
// Courses: { _id: 101, title: "Math", studentIds: [1, 2] }

// Or with junction collection:
// Enrollments: { _id: 1, studentId: 1, courseId: 101, grade: "A" }
```
**Two-way referencing** or a **junction collection** for additional metadata.

---

## Schema Design Patterns

### 1. Polymorphic Pattern
Documents have different fields but share the same collection.
```js
// Same collection, different structures
{ _id: 1, type: "car", doors: 4, engine: "v8" }
{ _id: 2, type: "bike", wheelSize: 26, frame: "aluminum" }
```

### 2. Attribute Pattern
Handle fields with many similar but sparse attributes.
```js
// Instead of: { product: "T-shirt", color_red: true, color_blue: true, size_s: false, size_m: true }
// Use:
{ product: "T-shirt", attributes: [
  { key: "color", value: "red" },
  { key: "size", value: "M" }
]}
```

### 3. Bucket Pattern
Group related data into time-based or size-based buckets.
```js
{ sensorId: "S1", readings: [
  { timestamp: ISODate("2024-01-01"), value: 72.5 },
  { timestamp: ISODate("2024-01-02"), value: 73.1 },
]}

// Instead of one document per reading
```
Useful for IoT, time-series, and log data.

### 4. Outlier Pattern
Handle documents with special cases that don't fit the common pattern.
```js
// Most users have a few addresses
// Outlier user has 1000+ addresses
{ _id: 1, name: "Alice", addresses: [...common...], hasManyAddresses: true }
// Addresses stored separately for this user
```

### 5. Computed Pattern
Pre-compute expensive calculations and store them.
```js
{ _id: 1, product: "Widget", price: 10, totalSales: 5000 }
// totalSales is pre-computed and updated periodically
```

### 6. Subset Pattern
Store frequently accessed data in main document, less-used data in separate collection.
```js
// Main document (frequently accessed)
{ _id: 1, name: "Movie A", year: 2024, rating: 8.5 }

// Reviews collection (infrequently accessed)
{ movieId: 1, reviews: [ ...10000 reviews... ] }
```

---

## BSON Data Types

| Type | Number | Example |
|------|--------|---------|
| Double | 1 | `3.14` |
| String | 2 | `"hello"` |
| Object | 3 | `{ a: 1 }` |
| Array | 4 | `[1, 2, 3]` |
| Binary Data | 5 | `BinData(...)` |
| ObjectId | 7 | `ObjectId("...")` |
| Boolean | 8 | `true` |
| Date | 9 | `ISODate("2024-01-01")` |
| Null | 10 | `null` |
| 32-bit Integer | 16 | `NumberInt(42)` |
| Timestamp | 17 | `Timestamp(0, 0)` |
| 64-bit Integer | 18 | `NumberLong(42)` |
| Decimal128 | 19 | `NumberDecimal("10.99")` |

---

## Schema Validation

MongoDB supports **schema validation** using JSON Schema (since v3.6):
```js
db.createCollection("users", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["name", "email", "age"],
      properties: {
        name: {
          bsonType: "string",
          description: "must be a string and is required"
        },
        email: {
          bsonType: "string",
          pattern: "^.+@.+$",
          description: "must be a valid email"
        },
        age: {
          bsonType: "int",
          minimum: 0,
          maximum: 150
        }
      }
    }
  },
  validationAction: "error"   // or "warn"
})
```

**Validation levels:**
- `"strict"` — Apply to all inserts and updates (default)
- `"moderate"` — Apply only to existing valid documents

---

## Best Practices Summary

1. **Design for your application's access patterns** — Not for relational normalization
2. **Embed for performance** — Embed data that is read together
3. **Reference for independence** — Reference data that is accessed separately
4. **Avoid `$lookup` on hot paths** — Joins are expensive
5. **Keep documents small** — Under 16MB limit, aim for KB range
6. **Plan for growth** — Consider how your data model scales
7. **Use arrays sparingly** — Unbounded arrays cause performance issues
8. **Prefer `$addToSet` over `$push`** — Avoid duplicates when appropriate
9. **Index your access patterns** — Create indexes for your most frequent queries
10. **Schema validate in production** — Use `$jsonSchema` for data quality
