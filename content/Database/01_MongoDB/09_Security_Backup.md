# MongoDB Security & Backup

## Authentication

### Authentication Mechanisms

| Mechanism | Description |
|-----------|-------------|
| **SCRAM** (default) | Challenge-response with username/password |
| **x.509 Certificates** | TLS/SSL certificate-based auth |
| **LDAP** | Enterprise — integrate with LDAP |
| **Kerberos** | Enterprise — integrate with Kerberos |
| **AWS IAM** | Atlas — authenticate via AWS IAM |

### Enable Authentication
```yaml
# mongod.conf
security:
  authorization: enabled
```

### Create User
```js
use admin
db.createUser({
  user: "adminUser",
  pwd: passwordPrompt(),          // or "password" string
  roles: [
    { role: "root", db: "admin" }
  ]
})

// Application user with specific privileges
db.createUser({
  user: "appUser",
  pwd: "securePassword123",
  roles: [
    { role: "readWrite", db: "myDatabase" },
    { role: "read", db: "logs" }
  ]
})
```

### Authentication in Connection String
```
mongodb://username:password@host:27017/database?authSource=admin
```

---

## Authorization & Roles

### Built-in Roles

| Role | Privileges |
|------|------------|
| `read` | Read data in database |
| `readWrite` | Read and write data |
| `dbAdmin` | Administer database (indexes, schema) |
| `dbOwner` | Combination of readWrite, dbAdmin, userAdmin |
| `userAdmin` | Manage users and roles |
| `clusterAdmin` | Administer cluster (sharding, replication) |
| `readAnyDatabase` | Read any database |
| `readWriteAnyDatabase` | Read/write any database |
| `userAdminAnyDatabase` | Manage users on any database |
| `dbAdminAnyDatabase` | Administer any database |
| `root` | Superuser (all privileges) |

### Custom Roles
```js
use admin
db.createRole({
  role: "customRole",
  privileges: [
    { resource: { db: "myDb", collection: "" }, actions: ["find", "insert", "update"] },
    { resource: { db: "myDb", collection: "reports" }, actions: ["find"] }
  ],
  roles: []
})
```

### View Users & Roles
```js
show users                                    // Current database users
db.getUsers()                                 // List users in current database
db.getUser("username")                        // Get specific user
db.getRole("roleName", { showPrivileges: true })  // Role details
```

---

## Network Security

### Bind IP
```yaml
# mongod.conf
net:
  bindIp: 127.0.0.1,10.0.0.1    # Listen on specific interfaces
  port: 27017
```

### TLS/SSL
```yaml
net:
  ssl:
    mode: requireSSL
    PEMKeyFile: /etc/ssl/mongodb.pem
    CAFile: /etc/ssl/ca.pem
```

### Firewall Rules
- Allow port `27017` only from trusted sources
- Use **VPN** or **VPC peering** for cloud deployments
- Prefer **private IPs** over public

---

## Encryption

### Encryption at Rest (Enterprise)
```yaml
security:
  enableEncryption: true
  encryptionKeyFile: /etc/mongodb/encryption-key
```

### Encryption in Transit (TLS)
```yaml
net:
  ssl:
    mode: requireSSL
    allowConnectionsWithoutCertificates: false
```

### Client-Side Field Level Encryption (FLE)
Encrypt specific fields before sending to MongoDB:
```js
const client = new MongoClient(uri, {
  autoEncryption: {
    kmsProviders: { local: { key: localKey } },
    keyVaultNamespace: "encryption.__keyVault"
  }
})
// Fields marked with encryption: { encrypt: ... } in JSON schema
// are automatically encrypted/decrypted by the driver
```

---

## Auditing (Enterprise)
Log all database operations for compliance:
```yaml
auditLog:
  destination: file
  format: JSON
  path: /var/log/mongodb/audit.log
  filter: '{ atype: "authenticate", "param.user": { $ne: "internal" } }'
```

---

## Backup & Restoration

### mongodump (BSON Backup)
```bash
# Backup all databases
mongodump --uri="mongodb://localhost:27017" --out=/backup/full

# Backup single database
mongodump --db=myDatabase --out=/backup/myDatabase

# Backup with authentication
mongodump --username=admin --password --authenticationDatabase=admin --db=myDb --out=/backup

# Backup specific collection
mongodump --db=myDb --collection=users --out=/backup
```

### mongorestore (BSON Restore)
```bash
# Restore all databases
mongorestore --uri="mongodb://localhost:27017" /backup/full

# Restore single database (drop existing)
mongorestore --db=myDatabase --drop /backup/full/myDatabase

# Restore specific collection
mongorestore --db=myDb --collection=users /backup/full/myDb/users.bson
```

### mongoexport (JSON/CSV Export)
```bash
# Export to JSON
mongoexport --db=myDb --collection=users --out=users.json

# Export to CSV
mongoexport --db=myDb --collection=users --type=csv --fields=name,email,age --out=users.csv

# Export with query
mongoexport --db=myDb --collection=users --query='{"age": {"$gte": 30}}' --out=users_filtered.json
```

### mongoimport (JSON/CSV Import)
```bash
# Import JSON
mongoimport --db=myDb --collection=users --file=users.json

# Import CSV
mongoimport --db=myDb --collection=users --type=csv --headerline --file=users.csv

# Drop collection before import
mongoimport --db=myDb --collection=users --drop --file=users.json
```

### Atlas Backup (Cloud)
- **Continuous backups** — Point-in-time recovery
- **Snapshot schedules** — Custom retention policies
- **Serverless backups** — Automated with Atlas serverless

---

## MongoDB Atlas

**Atlas** is MongoDB's fully managed cloud database service.

### Key Features
- Automated provisioning, scaling, and maintenance
- Multi-cloud (AWS, Azure, GCP)
- Global clusters (data distributed across regions)
- Automated backups with point-in-time recovery
- Built-in monitoring and alerts
- Serverless instances available
- Atlas Search (Lucene-based full-text search)
- Atlas Data Federation (query across multiple data sources)
- Online Archive (automatically archive old data)

### Connection String Format
```
mongodb+srv://cluster0.xxxxx.mongodb.net/myDatabase?retryWrites=true&w=majority
```

### IP Whitelist
Atlas requires IP whitelisting or VPC peering for security.

---

## Security Checklist
- [ ] Enable authentication
- [ ] Use strong passwords / keyfile authentication for replica sets
- [ ] Enable TLS/SSL for all connections
- [ ] Bind to specific IPs (not `0.0.0.0`)
- [ ] Create application-specific users with least privilege
- [ ] Enable auditing (Enterprise)
- [ ] Encrypt data at rest (Enterprise)
- [ ] Regular backup testing
- [ ] Monitor with Atlas or Ops Manager
- [ ] Keep MongoDB updated with latest patches
