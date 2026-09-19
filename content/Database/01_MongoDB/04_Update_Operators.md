# MongoDB Update Operators

## Field Update Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `$set` | Sets the value of a field | `{ $set: { age: 30 } }` |
| `$unset` | Removes a field | `{ $unset: { middle_name: "" } }` |
| `$inc` | Increments/decrements a numeric field | `{ $inc: { age: 1 } }` |
| `$mul` | Multiplies a numeric field | `{ $mul: { price: 1.1 } }` |
| `$min` | Updates if new value is less than current | `{ $min: { price: 99.99 } }` |
| `$max` | Updates if new value is greater than current | `{ $max: { price: 199.99 } }` |
| `$rename` | Renames a field | `{ $rename: { "oldName": "newName" } }` |
| `$currentDate` | Sets field to current date | `{ $currentDate: { lastModified: true } }` |
| `$setOnInsert` | Sets value only on insert (upsert) | `{ $setOnInsert: { createdAt: new Date() } }` |

### Examples
```js
// Increment age by 1
db.users.updateOne({ name: "Alice" }, { $inc: { age: 1 } })

// Set multiple fields
db.users.updateOne(
  { name: "Bob" },
  { $set: { age: 26, status: "active" } }
)

// Set field only if current value is lower
db.users.updateOne({ name: "Alice" }, { $min: { salary: 50000 } })

// Rename a field
db.users.updateOne({ name: "Alice" }, { $rename: { "email": "contact_email" } })

// Set current date
db.users.updateOne(
  { name: "Alice" },
  { $currentDate: { lastModified: true }, $set: { status: "updated" } }
)

// Use with upsert — set createdAt only on new document
db.users.updateOne(
  { name: "Frank" },
  { $set: { age: 40 } },
  { upsert: true, $setOnInsert: { createdAt: new Date() } }
)
```

---

## Array Update Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `$push` | Add element to array | `{ $push: { hobbies: "reading" } }` |
| `$pop` | Remove first (-1) or last (1) element | `{ $pop: { hobbies: 1 } }` |
| `$pull` | Remove all matching elements | `{ $pull: { hobbies: "reading" } }` |
| `$pullAll` | Remove all specified values | `{ $pullAll: { hobbies: ["a", "b"] } }` |
| `$addToSet` | Add element if not already present | `{ $addToSet: { tags: "mongodb" } }` |
| `$` | Update first matching array element | `{ "comments.$.text": "Updated" }` |
| `$[]` | Update all array elements | `{ "grades.$[]": 100 }` |
| `$[<identifier>]` | Update filtered array elements | `{ "elements.$[elem].status": "done" }` |

### Examples
```js
// Push to array
db.users.updateOne({ name: "Alice" }, { $push: { hobbies: "swimming" } })

// Push with modifiers
db.users.updateOne(
  { name: "Alice" },
  { $push: { scores: { $each: [85, 90, 95], $sort: -1, $slice: 3 } } }
)

// Remove last element
db.users.updateOne({ name: "Alice" }, { $pop: { hobbies: 1 } })

// Remove specific value
db.users.updateOne({ name: "Alice" }, { $pull: { hobbies: "reading" } })

// Add to set (no duplicates)
db.users.updateOne({ name: "Alice" }, { $addToSet: { tags: "developer" } })

// Update first matching array element (positional operator $)
db.users.updateOne(
  { name: "Alice", "comments._id": 1 },
  { $set: { "comments.$.text": "Updated comment" } }
)

// Update all array elements
db.users.updateOne({ name: "Alice" }, { $set: { "scores.$[]": 0 } })

// Update filtered array elements (arrayFilters)
db.users.updateOne(
  { name: "Alice" },
  { $set: { "items.$[elem].price": 9.99 } },
  { arrayFilters: [{ "elem.quantity": { $gte: 10 } }] }
)
```

---

## Array Update Modifiers (with $push)

| Modifier | Description | Example |
|----------|-------------|---------|
| `$each` | Push multiple values | `{ $push: { scores: { $each: [85, 90] } } }` |
| `$position` | Insert at specific position | `{ $push: { items: { $each: ["x"], $position: 0 } } }` |
| `$sort` | Sort array after push | `{ $push: { scores: { $each: [95], $sort: -1 } } }` |
| `$slice` | Trim array to size after push | `{ $push: { logs: { $each: ["new"], $slice: -10 } } }` |

---

## Aggregation Pipeline Update Operators (MongoDB 4.2+)

Since MongoDB 4.2, update operations can accept an **aggregation pipeline**:
```js
db.users.updateMany(
  { status: "active" },
  [
    { $set: { lastUpdated: "$$NOW", age: { $add: ["$age", 1] } } }
  ]
)
```

This allows:
- Computing updates using other field values
- Conditional updates
- Pipeline expressions in update operations
