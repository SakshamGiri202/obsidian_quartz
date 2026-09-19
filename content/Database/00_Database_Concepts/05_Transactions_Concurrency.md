# Transactions & Concurrency Control

## Transaction
A **transaction** is a logical unit of work that includes one or more database operations (read/write). It represents a real-world event.

### States of a Transaction
```
Active → Partially Committed → Committed
   ↓            ↓
 Failed →     Aborted
```

- **Active** — Initial state; transaction is executing
- **Partially Committed** — After final statement is executed
- **Committed** — Changes are permanently stored
- **Failed** — Transaction cannot proceed
- **Aborted** — Transaction rolled back, database restored to previous state

---

## ACID Properties

| Property | Description | Responsible Component |
|----------|-------------|----------------------|
| **Atomicity** | All-or-nothing: transaction completes fully or not at all. If any part fails, entire transaction rolls back. | Transaction Manager |
| **Consistency** | Database must remain in a valid state before and after transaction. All rules/constraints must be preserved. | Application Programmer |
| **Isolation** | Concurrent transactions execute independently. Changes of one are invisible to others until committed. | Concurrency Control Manager |
| **Durability** | Once committed, changes are permanent and survive system failures. Stored in non-volatile memory. | Recovery Manager |

### Example: Bank Transfer T = T1 + T2
- T1: Deduct $100 from Account X
- T2: Add $100 to Account Y

Without ACID:
- If T1 succeeds but T2 fails → $100 disappears (atomicity violation)
- If total before was $700, after must still be $700 (consistency)
- Two concurrent transfers must not interfere (isolation)
- After commit, a crash must not lose the changes (durability)

---

## Schedules

A **schedule** defines the order of execution of operations from multiple transactions.

### Types of Schedules

```
                ┌─────────────────────────────┐
                │        Serial Schedule      │
                └─────────────────────────────┘
                ┌─────────────────────────────┐
                │     Non-Serial Schedule     │
                ├─────────────────────────────┤
                │  ┌─ Conflict Serializable ──│
                │  ├─ View Serializable ──────│
                │  └─ Non-Serializable ───────│
                └─────────────────────────────┘
```

### 1. Serial Schedule
Transactions execute one after another (no interleaving).
- Always consistent
- Poor performance (no concurrency)

### 2. Non-Serial Schedule
Transactions execute in interleaved manner.
- Better performance
- Must be checked for correctness

### Serializability

#### Conflict Serializability
A schedule is conflict serializable if it can be converted to a serial schedule by swapping **non-conflicting operations**.

**Conflicting operations** occur when:
- They belong to **different transactions**
- They operate on the **same data item**
- At least one is a **write**

#### View Serializability
A schedule is view serializable if:
- Transactions read the same initial values as a serial schedule
- Transactions read values written by the same transactions
- Final writes are by the same transactions

*Note: Conflict-serializable ⊂ View-serializable*

### Types of Non-Serial Schedules (Based on Recoverability)

| Schedule Type | Description |
|---------------|-------------|
| **Recoverable** | A transaction commits only after all transactions whose values it read have committed |
| **Cascadeless** | A transaction only reads committed data (prevents cascading aborts) |
| **Strict** | A transaction cannot read/write data written by another transaction until that transaction commits/aborts |
| **Non-Recoverable** | A transaction commits after reading uncommitted data (MUST BE AVOIDED) |

**Hierarchy:**
```
Serial ⊂ Strict ⊂ Cascadeless ⊂ Recoverable
```

---

## Concurrency Control Protocols

### 1. Lock-Based Protocol
Uses locks to control concurrent access to data items.

#### Lock Types
| Lock Type | Symbol | Description |
|-----------|--------|-------------|
| **Shared Lock** | S-lock | Read only; multiple transactions can hold shared locks simultaneously |
| **Exclusive Lock** | X-lock | Read and write; only one transaction can hold an exclusive lock |

#### Lock Compatibility Matrix
| | S | X |
|---|---|---|
| **S** | ✅ Yes | ❌ No |
| **X** | ❌ No | ❌ No |

#### Two-Phase Locking (2PL)
**Phase 1 (Growing):** Transaction acquires locks (cannot release any)
**Phase 2 (Shrinking):** Transaction releases locks (cannot acquire any)

**Variants:**
- **Basic 2PL** — Releases locks after use; may produce cascading aborts
- **Strict 2PL** — Releases all locks at commit/abort time (most common)
- **Rigorous 2PL** — All locks held until commit/abort

*Note: 2PL guarantees conflict serializability but does NOT prevent deadlocks.*

### 2. Timestamp Ordering Protocol
Each transaction is assigned a unique timestamp. The protocol enforces:
- **Read(X):** If TS(T) < W_TS(X), reject (older transaction trying to read newer version)
- **Write(X):** If TS(T) < R_TS(X) or TS(T) < W_TS(X), reject

Ensures serializability without locks.

### 3. Multiversion Concurrency Control (MVCC)
- Keeps multiple versions of each data item
- Each write creates a new version with a timestamp
- Reads access the version matching the transaction's timestamp
- Avoids read-write conflicts
- Used in PostgreSQL, MySQL (InnoDB), Oracle

### 4. Validation (Optimistic) Protocol
Assumes conflicts are rare. Three phases:
1. **Read Phase** — Transaction reads data, operations on private copy
2. **Validation Phase** — Check for conflicts before commit
3. **Write Phase** — Apply changes permanently

### 5. Graph-Based Protocol
Uses a DAG (Directed Acyclic Graph) to define lock order.
- Transactions can only lock nodes following the graph order
- Prevents deadlocks but requires advance knowledge of access patterns

---

## Deadlock

### Conditions for Deadlock
1. **Mutual Exclusion** — Resources cannot be shared
2. **Hold and Wait** — Transaction holds resources while waiting for others
3. **No Preemption** — Resources cannot be forcibly taken
4. **Circular Wait** — A cycle of transactions waiting for each other

### Deadlock Handling
| Strategy | Description |
|----------|-------------|
| **Deadlock Prevention** | Ensure at least one condition cannot occur (e.g., acquire all locks at once) |
| **Deadlock Detection** | Periodically check for cycles in wait-for graph |
| **Deadlock Recovery** | Abort one or more transactions to break the cycle |

---

## Multiple Granularity Locking
Allows locking at different levels of granularity:
- Database → Table → Page → Row → Attribute

Uses **intention locks** (IS, IX, SIX) to signal intent to acquire finer-grained locks.
