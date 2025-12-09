# Reliability & Consistency

Guarantees and models for distributed systems.

---

## CAP Theorem

In the presence of network **Partition**, choose between:
- **Consistency:** All nodes see the same data
- **Availability:** Every request gets a response

**Reality:** You cannot have all three (Consistency, Availability, Partition Tolerance).

**When partition occurs:**
- **CP System:** Reject requests to maintain consistency (e.g., banking)
- **AP System:** Accept requests but may return stale data (e.g., DNS, caching)

---

## PACELC Extension

**If Partition:** Choose Availability or Consistency  
**Else (no partition):** Choose Latency or Consistency

Even without failures, you trade lower latency for weaker consistency.

**Examples:**
- **PA/EL:** Cassandra (available during partition, low latency normally)
- **PC/EC:** MongoDB (consistent during partition, consistent with higher latency)

---

## Consistency Models

### Strong Consistency
All reads return the most recent write.
- **Implementation:** Synchronous replication, locks, consensus
- **Trade-off:** Higher latency, lower availability
- **Use for:** Financial transactions, inventory, booking systems
- **Examples:** Traditional RDBMS, etcd, ZooKeeper

### Eventual Consistency
Updates propagate asynchronously; reads may be stale temporarily.
- **Implementation:** Async replication, conflict resolution
- **Trade-off:** Lower latency, higher availability, complexity
- **Use for:** Social feeds, DNS, caching, analytics
- **Examples:** DynamoDB (default), Cassandra, DNS

### Causal Consistency
Preserves cause-effect relationships; related operations seen in order.
- **Middle ground** between strong and eventual
- **Use for:** Collaborative editing, messaging
- **Examples:** Some distributed databases with vector clocks

### Read-Your-Writes Consistency
User sees their own writes immediately (but not necessarily others').
- **Use for:** User profiles, settings

---

## Quorum Systems

Require majority (or subset) of replicas to agree on operations.

**Formula:**
- **N** = total replicas
- **W** = write replicas
- **R** = read replicas

**Strong consistency when:** R + W > N

**Example:** N=3, W=2, R=2
- Write to 2 nodes, read from 2 nodes
- Guaranteed overlap ensures you read latest write

**Tuning:**
- **W=1, R=N:** Fast writes, slow reads
- **W=N, R=1:** Slow writes, fast reads
- **W=2, R=2 (N=3):** Balanced

---

## Consensus Algorithms

Achieve agreement across nodes despite failures.

### Raft
Leader-based consensus algorithm.
- **Leader** handles all writes and replicates to followers
- **Leader election** when leader fails
- **Log replication** ensures consistency
- **Easier to understand** than Paxos

### Paxos
Classic consensus algorithm (complex but proven).
- **Multi-Paxos** used in practice

**Use cases:**
- Leader election
- Distributed locks
- Configuration management
- Replicated state machines

**Examples:** etcd (Raft), ZooKeeper (Zab/Paxos-like), Consul (Raft)

---

## Conflict Resolution

When eventual consistency allows conflicts:

### Last Write Wins (LWW)
Use timestamp to pick winner.
- **Simple** but may lose data
- **Problem:** Clock skew

### Version Vectors
Track causality to merge updates.
- **Complex** but preserves data

### Application-Level Merge
Let application decide (e.g., shopping cart merge).

---

## Decision Matrix

| Requirement | Model | Example |
|-------------|-------|---------|
| Money, inventory | Strong | PostgreSQL |
| Social feeds | Eventual | Cassandra |
| User settings | Read-your-writes | DynamoDB |
| Collaborative docs | Causal | CRDTs |
| Leader election | Consensus | etcd, Raft |

---

## Quick Tips

- **Default to strong consistency** unless scale demands otherwise
- **Measure actual consistency needs** (do you really need instant updates?)
- **Use idempotency** to handle retries safely
- **Plan for split-brain** scenarios in partition-tolerant systems
- **Monitor replication lag** in eventual consistency setups

---

## References

- *Designing Data-Intensive Applications* by Martin Kleppmann (Chapters 5, 7, 9)
- [Jepsen Analyses](https://jepsen.io/) – Real-world consistency testing
- [Raft Consensus](https://raft.github.io/)
- [AWS DynamoDB Consistency](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadConsistency.html)

