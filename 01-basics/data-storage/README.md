# Data & Storage

Database systems and storage patterns for scalable applications.

---

## RDBMS (Relational Databases)

SQL databases with tables, rows, and ACID transactions.

**Examples:** PostgreSQL, MySQL, Oracle, SQL Server

**Strengths:**
- ACID guarantees (Atomicity, Consistency, Isolation, Durability)
- Strong consistency and complex queries (JOINs)
- Mature tooling and proven reliability

**Limitations:**
- Vertical scaling (harder to shard)
- Schema changes can be expensive
- Lower write throughput at massive scale

**Use for:** Financial systems, user accounts, inventory, order management

---

## NoSQL Databases

Non-relational databases optimized for specific access patterns.

### Key-Value Stores
Simple get/put by key.
- **Examples:** Redis, DynamoDB, Riak
- **Use for:** Caching, session storage, user preferences

### Document Stores
Store JSON-like documents.
- **Examples:** MongoDB, Couchbase, Firestore
- **Use for:** Content management, catalogs, user profiles

### Wide-Column Stores
Rows with dynamic columns.
- **Examples:** Cassandra, HBase, BigTable
- **Use for:** Time-series, logs, IoT data

### Graph Databases
Nodes and relationships.
- **Examples:** Neo4j, Amazon Neptune
- **Use for:** Social networks, recommendation engines, fraud detection

---

## Sharding (Partitioning)

Split data across multiple nodes by key.

**Strategies:**
- **Hash-based:** Hash(key) % N → consistent hashing
- **Range-based:** Users A-M on node1, N-Z on node2
- **Geographic:** US users on US servers

**Benefits:** Scale writes, distribute storage  
**Challenges:** Cross-shard queries, rebalancing, hot partitions

---

## Replication

Copy data across multiple nodes.

### Master-Slave (Primary-Replica)
- Master handles writes, replicas serve reads
- **Pros:** Simple, read scalability
- **Cons:** Single write bottleneck, replication lag

### Multi-Master
- Multiple nodes accept writes
- **Pros:** Write scalability, geo-distribution
- **Cons:** Conflict resolution needed

**Synchronous:** Wait for replicas (strong consistency, slower)  
**Asynchronous:** Don't wait (eventual consistency, faster)

---

## Indexing

Data structures that speed queries.

**Types:**
- **B-Tree:** Range queries, sorted access
- **Hash Index:** Exact matches only
- **Full-text:** Search engines (Elasticsearch)
- **Geospatial:** Location queries

**Trade-off:** Faster reads ↔ Slower writes + more storage

---

## Specialized Databases

### Time-Series Databases
Optimized for timestamped data.
- **Examples:** InfluxDB, TimescaleDB, Prometheus
- **Use for:** Metrics, monitoring, sensor data

### OLTP vs OLAP
- **OLTP (Online Transaction Processing):** Fast, short queries (e.g., order checkout)
- **OLAP (Online Analytical Processing):** Complex, long queries (e.g., revenue reports)

---

## Decision Matrix

| Need | Choose | Why |
|------|--------|-----|
| ACID transactions | RDBMS | Strong guarantees |
| Massive write scale | NoSQL (wide-column) | Horizontal scaling |
| Complex relationships | Graph DB | Efficient traversals |
| Fast key lookup | Key-value | O(1) access |
| Time-series data | Time-series DB | Compression, retention |

---

## Quick Tips

- **Start with RDBMS** unless you have specific NoSQL requirements
- **Denormalize for reads** when queries are complex/slow
- **Partition carefully** to avoid hot spots
- **Monitor replication lag** in async setups

---

## References

- *Designing Data-Intensive Applications* by Martin Kleppmann (Chapters 2-3)
- [AWS Database Decision Guide](https://aws.amazon.com/products/databases/)
- [Database of Databases](https://dbdb.io/)

