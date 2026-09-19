# Entity-Relationship (ER) Model

The **Entity-Relationship Model (ER Model)** is a conceptual model for designing a database. It represents the logical structure of a database, including entities, their attributes, and relationships between them.

## Database Design Process
1. **Gather requirements** — Ask questions to database users
2. **Create logical/conceptual design** — ER model plays a role here
3. **Physical database design** — Indexing, storage, etc.
4. **External design** — Views, access patterns

---

## Components of ER Diagram

### 1. Entity
A real-world object, concept, or thing about which data is stored.

**Types of Entities:**
- **Strong Entity** — Has a key attribute that uniquely identifies each instance. Does not depend on any other entity. Represented by a single rectangle.
- **Weak Entity** — Cannot be uniquely identified by its own attributes alone. Depends on a strong entity. Represented by a double rectangle. Its relationship with the strong entity is called an **identifying relationship** (double diamond).

**Entity Set** — Collection of all entities of a particular entity type.

### 2. Attributes
Properties that define an entity type.

**Types of Attributes:**

| Type | Description | ER Notation |
|------|-------------|-------------|
| **Key Attribute** | Uniquely identifies each entity | Oval with underline |
| **Composite Attribute** | Composed of multiple attributes (e.g., Address → Street, City, State) | Oval comprising ovals |
| **Multivalued Attribute** | Can have more than one value (e.g., Phone_No) | Double oval |
| **Derived Attribute** | Can be derived from other attributes (e.g., Age from DOB) | Dashed oval |
| **Simple Attribute** | Cannot be divided further | Single oval |

### 3. Relationship
A connection between entities. Represented by a diamond shape.

**Degree of Relationship:**
- **Unary/Recursive** — One entity set participates (e.g., person married to person)
- **Binary** — Two entity sets participate (most common)
- **Ternary** — Three entity sets participate
- **N-ary** — N entity sets participate

---

## Cardinality Constraints

Cardinality defines the maximum number of times an entity participates in a relationship.

### 1. One-to-One (1:1)
Each entity in each set participates at most once.
```
Employee ──── manages ──── Department
(1)                          (1)
```

### 2. One-to-Many (1:M)
One entity can be associated with multiple entities.
```
Department ──── has ──── Doctor
(1)                        (M)
```

### 3. Many-to-One (M:1)
Multiple entities relate to one entity.
```
Surgery ──── performed_by ──── Surgeon
(M)                              (1)
```

### 4. Many-to-Many (M:N)
Entities in all sets can participate multiple times.
```
Student ──── enrolled_in ──── Course
(M)                            (N)
```

---

## Participation Constraints

- **Total Participation** — Every entity in the set must participate in the relationship. Shown by a **double line**.
- **Partial Participation** — Entity may or may NOT participate in the relationship. Shown by a **single line**.

---

## Structural Constraints

### Generalization
The process of extracting common properties from a set of entities and creating a generalized entity (bottom-up approach). Example: Student and Faculty generalize to Person.

### Specialization
The process of defining sub-groups of an entity (top-down approach). Example: Person specializes into Student and Faculty.

### Aggregation
Treating a relationship as an entity for higher-level abstraction.
- Used when a relationship needs to participate in another relationship
- Treats the relationship set as an abstract entity

---

## How to Draw an ER Diagram
1. **Identify Entities** — Represent in rectangles and label
2. **Identify Relationships** — Connect entities with diamonds
3. **Add Attributes** — Attach ovals to entities
4. **Define Primary Keys** — Underline key attributes
5. **Remove Redundancies** — Eliminate unnecessary elements
6. **Review for Clarity** — Ensure it effectively conveys relationships

---

## Mapping ER Model to Relational Model

| ER Model | Relational Model |
|----------|-----------------|
| Entity | Table (Relation) |
| Attribute | Column |
| Tuple (Record) | Row |
| Key Attribute | Primary Key |
| Relationship | Foreign Key + Junction Table |
| Weak Entity | Table with foreign key to strong entity |
| Multivalued Attribute | Separate table |
| Composite Attribute | Multiple columns in the same table |
