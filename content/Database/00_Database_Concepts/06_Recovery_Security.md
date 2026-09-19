# Database Recovery & Security

## Database Recovery

**Recovery** is the process of restoring a database to a correct and consistent state after a failure. It ensures **Atomicity** and **Durability** (ACID properties).

### Types of Failures
| Failure Type | Examples |
|--------------|----------|
| **Transaction Failures** | Logical errors, system errors, deadlock |
| **System Failures** | Power outage, OS crash, DBMS bug |
| **Media Failures** | Disk head crash, disk failure, physical damage |

---

## Recovery Techniques

### 1. Log-Based Recovery
The DBMS maintains a **log file** (journal) on stable storage that records every change (insert, update, delete) before it is applied to the database.

**Log Record Types:**
- `<Tn, START>` — Transaction Tn has started
- `<Tn, X, V1, V2>` — Tn changed X from V1 to V2
- `<Tn, COMMIT>` — Tn has committed
- `<Tn, ABORT>` — Tn has aborted

**Recovery Operations:**

| Operation | Description |
|-----------|-------------|
| **Undo** | Reverse changes from uncommitted transactions (ensures atomicity) |
| **Redo** | Reapply changes from committed transactions (ensures durability) |

**Approaches:**
- **Immediate Update (Undo/Redo)** — Database may be updated before commit. On failure, undo uncommitted and redo committed.
- **Deferred Update (No-Undo/Redo)** — Updates applied only after commit. On failure, only redo is needed.

### 2. Shadow Paging
Alternative to log-based recovery that avoids a log file.

- Maintains two page tables: **current** and **shadow**
- Shadow page table points to unmodified pages (consistent state before transaction)
- Current page table points to newly modified pages
- On commit: current becomes the new shadow
- On abort: discard current, revert to shadow
- **Advantage:** No log needed, simple recovery
- **Disadvantage:** Storage fragmentation, harder to manage

### 3. Checkpointing
An optimization that makes recovery faster.

- Periodically writes all log records and modified pages from memory to stable storage
- Creates a **checkpoint record** in the log
- On recovery, only scans from the last checkpoint (not the entire log)
- Significantly speeds up recovery

### 4. Backup and Restore
The last safeguard against severe failures.

| Backup Type | Description |
|-------------|-------------|
| **Full Backup** | Complete copy of entire database |
| **Differential Backup** | Copy of data changed since last full backup |
| **Transaction Log Backup** | Copy of transaction log for point-in-time recovery |

---

## ARIES Recovery Algorithm
**Algorithm for Recovery and Isolation Exploiting Semantics** — the most widely used recovery method.

**Three phases:**
1. **Analysis** — Determine dirty pages and transactions active at crash time
2. **Redo** — Reapply all changes from the log (starting from the earliest dirty page)
3. **Undo** — Rollback uncommitted transactions in reverse order

**Key concepts:**
- **WAL (Write-Ahead Logging)** — Log must be written before data is written to disk
- **STEAL** — Buffer manager can write uncommitted data to disk
- **NO-FORCE** — Committed data doesn't need to be written to disk immediately

---

## Database Security

### Security Goals (CIA Triad)
1. **Confidentiality** — Only authorized users can access data
2. **Integrity** — Data is accurate and complete
3. **Availability** — Data is accessible when needed

### Access Control Mechanisms

| Mechanism | Description |
|-----------|-------------|
| **Authentication** | Verifying user identity (passwords, biometrics, MFA) |
| **Authorization** | Determining what a user can do (privileges, roles) |
| **DCL (GRANT/REVOKE)** | SQL commands for managing permissions |

### Types of Privileges
- **System Privileges** — Create/alter/drop database objects
- **Object Privileges** — Select/insert/update/delete on specific objects

### Security Threats
| Threat | Description |
|--------|-------------|
| **SQL Injection** | Malicious SQL inserted into application queries |
| **Unauthorized Access** | Access without proper credentials |
| **Data Breach** | Sensitive data exposed |
| **Insider Threats** | Authorized users misusing access |
| **Denial of Service (DoS)** | Making database unavailable |

### Security Measures
- **Encryption** — Data encrypted at rest and in transit
- **Auditing** — Tracking who accessed what and when
- **Views** — Restrict data access through limited views
- **Firewalls** — Network-level protection
- **Parameterized Queries** — Prevent SQL injection
- **Least Privilege Principle** — Minimum necessary access
