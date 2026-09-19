# Relational Model

## Introduction
The **Relational Model** organizes data into **tables (relations)** with rows (tuples) and columns (attributes). It was proposed by **E.F. Codd** in 1970.

### Key Terminology
| Term                  | Definition                                            |
| --------------------- | ----------------------------------------------------- |
| **Relation**          | A table with rows and columns                         |
| **Tuple**             | A row in a relation                                   |
| **Attribute**         | A column in a relation                                |
| **Domain**            | Set of allowable values for an attribute              |
| **Degree**            | Number of attributes in a relation                    |
| **Cardinality**       | Number of tuples in a relation                        |
| **Relation Schema**   | Name + attributes (e.g., Student(roll_no, name, age)) |
| **Relation Instance** | Actual set of tuples at a given time                  |

---

## Codd's 12 Rules for RDBMS
1. **Information Rule** — All data must be represented as table values
2. **Guaranteed Access Rule** — Every value must be accessible by table name, primary key, and column name
3. **Systematic Treatment of NULLs** — NULLs must be handled uniformly
4. **Dynamic Online Catalog** — Metadata must be stored as relations
5. **Comprehensive Data Sublanguage** — At least one language (e.g., SQL) supporting data definition, manipulation, integrity, and transactions
6. **View Updating Rule** — All views must be updatable
7. **High-Level Insert, Update, Delete** — Set-level operations
8. **Physical Data Independence** — Changes to physical storage don't affect logical schema
9. **Logical Data Independence** — Changes to logical schema don't affect external views
10. **Integrity Independence** — Integrity constraints must be separable from application programs
11. **Distribution Independence** — Database distribution doesn't affect applications
12. **Non-Subversion Rule** — Low-level interface cannot bypass integrity rules

---

## Keys in Relational Model

| Key Type | Description |
|----------|-------------|
| **Super Key** | Set of attributes that uniquely identifies a tuple (may contain extra attributes) |
| **Candidate Key** | Minimal super key (no proper subset is a super key) |
| **Primary Key** | Selected candidate key; uniquely identifies each tuple; cannot be NULL |
| **Foreign Key** | Attribute in one relation referring to primary key of another relation |
| **Alternate Key** | Candidate keys not chosen as primary key |
| **Composite Key** | Combination of two or more attributes forming a primary key |
| **Unique Key** | Ensures uniqueness; allows one NULL value |
| **Surrogate Key** | Artificial key (auto-increment ID) used when no natural key exists |

---

## Integrity Constraints

### 1. Domain Constraints
- Every attribute value must be atomic and from its specified domain
- Defined by data types (INTEGER, VARCHAR, DATE, etc.)

### 2. Entity Integrity Constraints
- **Primary key cannot be NULL**
- Ensures each tuple is uniquely identifiable

### 3. Referential Integrity Constraints
- Foreign key values must match a primary key value in the referenced relation (or be NULL)
- Ensures consistency between related tables

### 4. Key Constraints (Uniqueness Constraints)
- Every tuple in a relation must be unique
- Candidate keys ensure no duplicate tuples

### 5. NOT NULL Constraint
- Prevents NULL values in specified columns

### 6. CHECK Constraint
- Validates data against a specified condition (e.g., age > 18)

### 7. DEFAULT Constraint
- Provides a default value when no value is specified

---

## Relational Algebra

Relational Algebra is a **procedural query language** that takes relations as input and produces a relation as output.

### Basic Operations

| Operation | Symbol | Description |
|-----------|--------|-------------|
| **Select** | σ (sigma) | Selects rows satisfying a condition: σ_condition(R) |
| **Project** | π (pi) | Selects specific columns: π_col1,col2(R) |
| **Union** | ∪ | All tuples from both relations (must be union-compatible) |
| **Set Difference** | − | Tuples in first but not in second |
| **Cartesian Product** | × | Combines every tuple from R with every tuple from S |
| **Rename** | ρ (rho) | Renames relation or attribute |

### Extended Operations

| Operation | Description |
|-----------|-------------|
| **Intersection** (∩) | Tuples present in both relations |
| **Natural Join** (⋈) | Combines relations on common attributes |
| **Theta Join** (⋈_θ) | Combines relations on a condition |
| **Left/Right/Full Outer Join** | Preserves unmatched tuples with NULLs |
| **Division** (÷) | Finds tuples in R associated with all tuples in S |

### SQL Equivalents of Relational Algebra
- **σ (Select)** → `WHERE` clause
- **π (Project)** → `SELECT` columns
- **∪ (Union)** → `UNION`
- **− (Difference)** → `EXCEPT`
- **× (Product)** → `CROSS JOIN`
- **⋈ (Join)** → `JOIN ... ON`

---

## Relational Calculus

Relational Calculus is a **non-procedural query language** — it describes WHAT to retrieve, not HOW.

### Tuple Relational Calculus (TRC)
- Uses tuple variables
- Format: `{ t | CONDITION(t) }`
- Example: `{ t | t ∈ Employee ∧ t.salary > 50000 }`

### Domain Relational Calculus (DRC)
- Uses domain variables (attribute values)
- Format: `{ <x1, x2, ..., xn> | CONDITION(x1, x2, ..., xn) }`
- Example: `{ <name, salary> | ∃ dept (Employee(name, salary, dept) ∧ dept = 'IT') }`

---

## SQL (Structured Query Language)

### DDL (Data Definition Language)
| Command | Description |
|---------|-------------|
| `CREATE` | Create database objects (tables, views, indexes) |
| `ALTER` | Modify database structure |
| `DROP` | Delete database objects |
| `TRUNCATE` | Remove all records from a table |
| `RENAME` | Rename database objects |

### DML (Data Manipulation Language)
| Command | Description |
|---------|-------------|
| `SELECT` | Retrieve data |
| `INSERT` | Add new rows |
| `UPDATE` | Modify existing data |
| `DELETE` | Remove rows |

### DCL (Data Control Language)
| Command | Description |
|---------|-------------|
| `GRANT` | Give user access privileges |
| `REVOKE` | Remove user access privileges |

### TCL (Transaction Control Language)
| Command | Description |
|---------|-------------|
| `COMMIT` | Save transaction permanently |
| `ROLLBACK` | Undo transaction changes |
| `SAVEPOINT` | Set a savepoint within a transaction |

### SQL Join Types
- **INNER JOIN** — Returns matching rows from both tables
- **LEFT JOIN** — All rows from left table, matching from right (NULLs for non-matches)
- **RIGHT JOIN** — All rows from right table, matching from left
- **FULL OUTER JOIN** — All rows from both tables
- **CROSS JOIN** — Cartesian product
- **NATURAL JOIN** — Join on all common columns
- **SELF JOIN** — Join a table with itself

### SQL Set Operations
- `UNION` — Combines results, removes duplicates
- `UNION ALL` — Combines results, keeps duplicates
- `INTERSECT` — Common rows
- `EXCEPT` / `MINUS` — Rows in first not in second

### Aggregate Functions
`COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()`

### Group By & Having
- `GROUP BY` — Groups rows with same values
- `HAVING` — Filters groups (like WHERE for groups)

### Subqueries
- Nested queries inside SELECT, FROM, or WHERE clauses
- Can be correlated (references outer query) or non-correlated
