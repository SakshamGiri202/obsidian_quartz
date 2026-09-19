# Normalization & Functional Dependencies

## What is Normalization?
**Normalization** is the process of organizing data in a database to reduce redundancy and eliminate data anomalies. It involves splitting tables into multiple tables and defining relationships between them.

### Goals of Normalization
- Eliminate data redundancy
- Avoid insert, update, and delete anomalies
- Ensure data consistency and integrity
- Make the database easier to maintain

### Data Anomalies
| Anomaly | Description |
|---------|-------------|
| **Insertion Anomaly** | Cannot insert data because other related data is missing |
| **Update Anomaly** | Updating data requires multiple changes due to redundancy |
| **Deletion Anomaly** | Deleting a row removes unintended data |

---

## Functional Dependencies (FD)

A **Functional Dependency** X → Y means that the value of X uniquely determines the value of Y.

- X is the **determinant** (left-hand side)
- Y is the **dependent** (right-hand side)
- Notation: X → Y (read as "X functionally determines Y")

### Types of Functional Dependencies
- **Trivial FD** — Y ⊆ X (e.g., A → A, AB → A)
- **Non-Trivial FD** — Y ⊈ X (e.g., A → B)
- **Completely Non-Trivial FD** — X ∩ Y = ∅

### Armstrong's Axioms (Inference Rules)
1. **Reflexivity** — If Y ⊆ X, then X → Y
2. **Augmentation** — If X → Y, then XZ → YZ
3. **Transitivity** — If X → Y and Y → Z, then X → Z

### Additional Rules (Derived)
4. **Union** — If X → Y and X → Z, then X → YZ
5. **Decomposition** — If X → YZ, then X → Y and X → Z
6. **Pseudo-Transitivity** — If X → Y and WY → Z, then WX → Z

### Attribute Closure
The closure of attribute set X (denoted X⁺) is the set of all attributes functionally determined by X.

**Algorithm:**
```
result = X
while (changes) {
    for each FD (α → β) {
        if α ⊆ result, result = result ∪ β
    }
}
```

**Uses:**
- Check if X is a super key (X⁺ contains all attributes)
- Check if FD X → Y holds (Y ⊆ X⁺)
- Find candidate keys

### Canonical Cover (Minimal Cover)
The minimal set of FDs that is equivalent to the original set. Conditions:
1. Each FD has a single attribute on the right side
2. No FD can be removed without changing closure
3. No attribute in the left side can be removed without changing closure

---

## Normal Forms

### 1NF (First Normal Form)
**Rule:** Every attribute must contain **atomic (single-valued)** values. No multi-valued attributes or repeating groups.

```
❌ Student(roll_no, name, phones)
   (1, 'Alice', {12345, 67890})

✅ Student(roll_no, name, phone)
   (1, 'Alice', 12345)
   (1, 'Alice', 67890)
```

### 2NF (Second Normal Form)
**Rule:** Must be in 1NF + **No partial dependency** — every non-prime attribute must be fully functionally dependent on the entire primary key (not just part of it).

*Partial dependency occurs when a non-key attribute depends on only part of a composite primary key.*

### 3NF (Third Normal Form)
**Rule:** Must be in 2NF + **No transitive dependency** — no non-prime attribute should depend on another non-prime attribute.

A relation is in 3NF if for every non-trivial FD X → Y:
- X is a super key, **OR**
- Y is a prime attribute (part of a candidate key)

### BCNF (Boyce-Codd Normal Form)
**Rule:** Must be in 3NF + For every non-trivial FD X → Y, **X must be a super key**.

BCNF is stricter than 3NF. Every BCNF relation is also in 3NF, but not vice versa.

### 4NF (Fourth Normal Form)
**Rule:** Must be in BCNF + **No multi-valued dependencies (MVD)**.

A multi-valued dependency X ↠ Y exists when:
- For a given X value, there is a set of Y values
- Y values are independent of other attributes

### 5NF (Fifth Normal Form / Project-Join Normal Form)
**Rule:** Must be in 4NF + **No join dependency** — every join dependency must be implied by candidate keys.

The table cannot be further decomposed without losing information.

---

## Normal Form Hierarchy
```
5NF ⊂ 4NF ⊂ BCNF ⊂ 3NF ⊂ 2NF ⊂ 1NF
```

Each higher normal form is a subset of the previous one.

---

## Important Concepts

### Prime vs Non-Prime Attributes
- **Prime Attribute** — An attribute that is part of any candidate key
- **Non-Prime Attribute** — An attribute that is not part of any candidate key

### Decomposition
Splitting a relation into multiple relations.

**Types:**
1. **Lossless Join Decomposition** — Joining the decomposed relations yields exactly the original relation (no spurious tuples)
2. **Dependency Preserving Decomposition** — All FDs can be checked on the decomposed relations without needing joins

### Denormalization
The intentional introduction of redundancy to improve read performance. Used when:
- Read-heavy workloads
- Complex queries need to avoid frequent joins
- OLAP/data warehouse scenarios

### The Problem of Redundancy
- Wastes storage space
- Causes update anomalies
- Leads to inconsistent data
- Increases maintenance complexity
