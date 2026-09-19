# MongoDB Aggregation Framework

## Overview
The **Aggregation Framework** processes documents through a **pipeline** of stages. Each stage transforms documents, and the output of one stage is the input to the next.

```
documents → $match → $group → $sort → $project → result
```

### When to Use Aggregation
- Complex data analysis and transformations
- Grouping and computing statistics
- Reshaping documents
- Joining collections ($lookup)
- Time-series analysis

---

## Aggregation Approaches

### 1. Single-Purpose Aggregation
Simple functions for basic analytics:
```js
db.users.countDocuments({ age: { $gt: 25 } })      // Count
db.users.estimatedDocumentCount()                    // Fast estimated count
db.users.distinct("city")                            // Unique values
```

### 2. Aggregation Pipeline
Powerful multi-stage processing:
```js
db.collection.aggregate([
  { stage1 },
  { stage2 },
  ...
])
```

---

## Pipeline Stages

### `$match`
Filters documents (like `find`). Should be placed early in pipeline.
```js
db.users.aggregate([
  { $match: { age: { $gte: 25 }, status: "active" } }
])
```

### `$project`
Reshapes documents (include/exclude fields, add computed fields).
```js
db.users.aggregate([
  { $project: {
      name: 1,
      email: 1,
      _id: 0,
      fullName: { $concat: ["$firstName", " ", "$lastName"] },
      isAdult: { $gte: ["$age", 18] }
  }}
])
```

### `$group`
Groups documents by a specified key and computes accumulators.
```js
db.users.aggregate([
  { $group: {
      _id: "$city",             // Group by city
      totalUsers: { $sum: 1 },  // Count users per city
      avgAge: { $avg: "$age" }, // Average age per city
      maxAge: { $max: "$age" }, // Max age per city
      names: { $push: "$name" } // Collect names into array
  }}
])
```

#### Accumulators in $group

| Accumulator | Description |
|-------------|-------------|
| `$sum` | Sum of values (or count if `1`) |
| `$avg` | Average of values |
| `$min` | Minimum value |
| `$max` | Maximum value |
| `$first` | First value in group |
| `$last` | Last value in group |
| `$push` | Collect values into array |
| `$addToSet` | Collect unique values into array |
| `$stdDevPop` | Population standard deviation |
| `$stdDevSamp` | Sample standard deviation |

### `$sort`
Sorts documents (1 = ascending, -1 = descending).
```js
db.users.aggregate([
  { $sort: { age: -1, name: 1 } }
])
```

### `$limit`
Limits the number of documents passed to next stage.
```js
db.users.aggregate([
  { $limit: 10 }
])
```

### `$skip`
Skips a specified number of documents.
```js
db.users.aggregate([
  { $skip: 20 }
])
```

### `$unwind`
Deconstructs an array field, creating a document for each element.
```js
// Document: { name: "Alice", hobbies: ["reading", "coding"] }
db.users.aggregate([
  { $unwind: "$hobbies" }
])
// Result: { name: "Alice", hobbies: "reading" }
//         { name: "Alice", hobbies: "coding" }
```

Options:
```js
{ $unwind: { path: "$tags", preserveNullAndEmptyArrays: true } }
```

### `$lookup`
Performs a left outer join with another collection.
```js
db.orders.aggregate([
  { $lookup: {
      from: "products",         // Target collection
      localField: "productId",  // Field from input docs
      foreignField: "_id",      // Field from target collection
      as: "product"             // Output array field
  }},
  { $unwind: "$product" }       // Deconstruct array to object
])
```

With pipeline (MongoDB 3.6+):
```js
db.orders.aggregate([
  { $lookup: {
      from: "products",
      let: { pid: "$productId" },
      pipeline: [
        { $match: { $expr: { $eq: ["$_id", "$$pid"] } } },
        { $project: { name: 1, price: 1 } }
      ],
      as: "product"
  }}
])
```

### `$addFields`
Adds new fields to documents.
```js
db.users.aggregate([
  { $addFields: { fullName: { $concat: ["$first", " ", "$last"] } } }
])
```

### `$count`
Returns a count of the remaining documents.
```js
db.users.aggregate([
  { $match: { age: { $gte: 30 } } },
  { $count: "totalUsers" }
])
// Returns: { totalUsers: 42 }
```

### `$out`
Writes pipeline results to a collection.
```js
db.users.aggregate([
  { $group: { _id: "$city", total: { $sum: 1 } } },
  { $out: "citySummary" }
])
```

### `$merge`
Merges pipeline results into a collection (MongoDB 4.4+). More flexible than `$out` — supports insert, merge, replace, keepExisting, fail.
```js
db.users.aggregate([
  { $group: { _id: "$city", total: { $sum: 1 } } },
  { $merge: { into: "citySummary", on: "_id", whenMatched: "merge" } }
])
```

### `$bucket`
Categorizes documents into buckets (ranges).
```js
db.users.aggregate([
  { $bucket: {
      groupBy: "$age",
      boundaries: [0, 18, 30, 50, 100],
      default: "Other",
      output: { count: { $sum: 1 }, names: { $push: "$name" } }
  }}
])
```

### `$facet`
Processes multiple aggregation pipelines in a single stage.
```js
db.products.aggregate([
  { $facet: {
      categoryCounts: [{ $group: { _id: "$category", count: { $sum: 1 } } }],
      priceStats: [{ $group: { _id: null, avg: { $avg: "$price" }, max: { $max: "$price" } } }]
  }}
])
```

---

## Expression Operators

### String Expressions
```js
{ $concat: ["$first", " ", "$last"] }
{ $toUpper: "$name" }
{ $toLower: "$name" }
{ $substrCP: ["$text", 0, 10] }    // Substring
{ $strLenCP: "$name" }              // String length
{ $trim: { input: "$name" } }
```

### Arithmetic Expressions
```js
{ $add: ["$price", "$tax"] }
{ $subtract: ["$price", "$discount"] }
{ $multiply: ["$quantity", "$price"] }
{ $divide: ["$total", "$count"] }
{ $mod: ["$amount", 100] }
{ $abs: "$value" }
{ $round: ["$value", 2] }
```

### Date Expressions
```js
{ $year: "$createdAt" }
{ $month: "$createdAt" }
{ $dayOfMonth: "$createdAt" }
{ $dayOfWeek: "$createdAt" }
{ $hour: "$createdAt" }
{ $dateToString: { format: "%Y-%m-%d", date: "$createdAt" } }
{ $dateFromParts: { year: "$year", month: "$month", day: "$day" } }
```

### Conditional Expressions
```js
{ $cond: { if: { $gte: ["$age", 18] }, then: "Adult", else: "Minor" } }
{ $ifNull: ["$middleName", "N/A"] }
{ $switch: {
    branches: [
      { case: { $lt: ["$score", 60] }, then: "F" },
      { case: { $lt: ["$score", 70] }, then: "D" }
    ],
    default: "A"
}}
```

### Array Expressions
```js
{ $arrayElemAt: ["$tags", 0] }     // Get element at index
{ $first: "$tags" }                 // First element
{ $last: "$tags" }                  // Last element
{ $slice: ["$items", 2, 5] }       // Slice array
{ $size: "$tags" }                  // Array length
{ $in: ["search", "$tags"] }       // Check if in array
{ $filter: { input: "$scores", as: "s", cond: { $gte: ["$$s", 80] } } }
{ $map: { input: "$items", as: "item", in: { $multiply: ["$$item.price", 1.1] } } }
{ $reduce: { input: "$nums", initialValue: 0, in: { $add: ["$$value", "$$this"] } } }
```

---

## Map-Reduce

Legacy approach for large-scale data processing (prefer aggregation pipeline).
```js
db.orders.mapReduce(
  function() { emit(this.customerId, this.amount); },     // map
  function(key, values) { return Array.sum(values); },     // reduce
  { out: "customerTotals", query: { status: "shipped" } } // options
)
```

**Note:** Aggregation pipeline is preferred over Map-Reduce since MongoDB 3.2+ for most use cases.

---

## Performance Tips
- **Place `$match` early** — Reduces documents flowing through pipeline
- **Place `$project` early** — Reduces fields flowing through pipeline
- **Use indexes** — `$match` and `$sort` can use indexes
- **Avoid `$unwind`** — Causes document multiplication
- **Use `$lookup` efficiently** — Ensure foreign field is indexed
- **Use `$out` or `$merge`** — Materialize intermediate results for repeated use
- **Limit results** — Use `$limit` to reduce final document count
