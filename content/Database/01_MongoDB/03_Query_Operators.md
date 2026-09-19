# MongoDB Query & Projection Operators

MongoDB provides a rich set of operators for building expressive queries.

---

## Comparison Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| `$eq` | Equal to | `{ age: { $eq: 30 } }` |
| `$ne` | Not equal to | `{ age: { $ne: 30 } }` |
| `$gt` | Greater than | `{ age: { $gt: 25 } }` |
| `$gte` | Greater than or equal | `{ age: { $gte: 25 } }` |
| `$lt` | Less than | `{ age: { $lt: 35 } }` |
| `$lte` | Less than or equal | `{ age: { $lte: 35 } }` |
| `$in` | In array | `{ name: { $in: ["Alice", "Bob"] } }` |
| `$nin` | Not in array | `{ name: { $nin: ["Charlie"] } }` |

```js
// Find users aged 25-35 inclusive
db.users.find({ age: { $gte: 25, $lte: 35 } })

// Find users not in specific cities
db.users.find({ city: { $nin: ["NYC", "LA"] } })
```

---

## Logical Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| `$and` | All conditions must be true | `{ $and: [{ age: { $gt: 25 } }, { age: { $lt: 35 } }] }` |
| `$or` | At least one condition true | `{ $or: [{ name: "Alice" }, { age: 30 }] }` |
| `$not` | Invert condition | `{ age: { $not: { $gt: 30 } } }` |
| `$nor` | None of the conditions true | `{ $nor: [{ name: "Alice" }, { age: 30 }] }` |

**Note:** Implicit AND is achieved by comma-separated conditions in the query document:
```js
{ age: { $gt: 25 }, name: "Alice" }   // Implicit AND (same as $and)
```

---

## Element Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| `$exists` | Field exists (or not) | `{ email: { $exists: true } }` |
| `$type` | Field is of specified BSON type | `{ name: { $type: "string" } }` |

```js
db.users.find({ email: { $exists: false } })          // Users without email field
db.users.find({ age: { $type: "int" } })              // age is integer type
```

### BSON Types
`double`, `string`, `object`, `array`, `binData`, `objectId`, `bool`, `date`, `null`, `regex`, `int`, `long`, `decimal`, `timestamp`

---

## Evaluation Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| `$regex` | Pattern matching | `{ name: { $regex: "^A", $options: "i" } }` |
| `$expr` | Use aggregation expressions | `{ $expr: { $gt: ["$price", "$discount"] } }` |
| `$mod` | Modulo operation | `{ age: { $mod: [5, 0] } }` (age % 5 === 0) |
| `$text` | Text search | `{ $text: { $search: "mongodb tutorial" } }` |
| `$where` | JavaScript expression | `{ $where: "this.age > 25" }` |

### Regex Examples
```js
db.users.find({ name: { $regex: /^A/i } })            // Names starting with A (case-insensitive)
db.users.find({ name: { $regex: "son$" } })            // Names ending with "son"
db.users.find({ email: { $regex: /@gmail\.com$/ } })   // Gmail users
```

### $expr Example
```js
// Find products where price > discounted_price
db.products.find({ $expr: { $gt: ["$price", "$discounted_price"] } })
```

---

## Array Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| `$all` | Array contains all elements | `{ skills: { $all: ["JavaScript", "MongoDB"] } }` |
| `$elemMatch` | Array element matches all conditions | `{ scores: { $elemMatch: { $gte: 85, $lte: 95 } } }` |
| `$size` | Array has exact length | `{ tags: { $size: 3 } }` |

```js
// Find documents where array field has specific element
db.users.find({ hobbies: "reading" })

// Find documents where array has ALL specified elements
db.users.find({ skills: { $all: ["Java", "Python"] } })

// Find documents where at least one array element matches ALL conditions
db.users.find({ scores: { $elemMatch: { subject: "Math", score: { $gte: 90 } } } })

// Find documents where array has exactly N elements
db.users.find({ tags: { $size: 5 } })
```

---

## Projection Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| `$` | First array element matching condition | `{ "comments.$": 1 }` |
| `$elemMatch` | Project matching array elements | `{ scores: { $elemMatch: { score: { $gte: 90 } } } }` |
| `$slice` | Slice array elements | `{ comments: { $slice: 5 } }` |
| `$meta` | Text search metadata | `{ score: { $meta: "textScore" } }` |

### Projection Examples
```js
// Return only first matching element in array
db.users.find(
  { hobbies: "reading" },
  { "hobbies.$": 1 }
)

// Return only last 3 comments
db.users.find(
  {},
  { comments: { $slice: -3 } }
)

// Return text search relevance score
db.users.find(
  { $text: { $search: "mongodb" } },
  { score: { $meta: "textScore" } }
).sort({ score: { $meta: "textScore" } })

// Return array elements matching condition
db.users.find(
  {},
  { scores: { $elemMatch: { score: { $gte: 90 } } } }
)
```

---

## Geospatial Operators

| Operator | Meaning |
|----------|---------|
| `$near` | Near a point (sorted by distance) |
| `$geoWithin` | Within a geometry |
| `$geoIntersects` | Intersects a geometry |
| `$nearSphere` | Near a point on a sphere |

---

## Bitwise Operators

| Operator | Meaning |
|----------|---------|
| `$bitsAllClear` | All bits clear |
| `$bitsAllSet` | All bits set |
| `$bitsAnyClear` | Any bit clear |
| `$bitsAnySet` | Any bit set |
