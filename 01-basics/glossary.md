# Quick Glossary

Fast reference for common system design terms.

---

## Core Terms

**Idempotency** – Operation can be repeated multiple times with same result (e.g., PUT request)

**Hot Partition / Hotspot** – Single partition receiving disproportionate traffic, causing bottleneck

**Sticky Session** – User requests always routed to same server (breaks stateless design)

**Cold Start** – Slow first-time initialization cost (common in serverless, JVM apps)

**Thundering Herd** – Many clients retry simultaneously, overwhelming service

**Split Brain** – Network partition causes multiple nodes to think they're the leader

**Data Locality** – Keep related data close (same server, region) to reduce latency

---

## Communication Patterns

**Fan-Out** – One message triggers multiple downstream operations
- Example: User posts → notify all followers

**Fan-In** – Multiple sources feed into one destination
- Example: Aggregate logs from all servers

**Request-Reply** – Synchronous call waiting for response

**Fire-and-Forget** – Async message with no response expected

**Pub-Sub** – Publishers send to topic, subscribers receive independently

---

## Failure Modes

**Cascading Failure** – One component failure triggers chain of failures

**Fail-Fast** – Detect and report errors immediately rather than retry

**Graceful Degradation** – Reduce functionality instead of total failure
- Example: Show cached data when DB is down

**Bulkhead** – Isolate failures to prevent spread (like ship compartments)

**Circuit Breaker** – Stop calling failing service to prevent cascade

---

## Data Patterns

**Write-Through** – Write to cache and DB synchronously

**Write-Back** – Write to cache, async write to DB (faster but risk of loss)

**Cache-Aside** – App checks cache, loads from DB on miss

**Read-Through** – Cache loads from DB automatically on miss

**Denormalization** – Duplicate data to avoid JOINs (faster reads, slower writes)

**Materialized View** – Pre-computed query results stored as table

---

## Scaling Terms

**Vertical Scaling (Scale Up)** – Bigger machine (more CPU/RAM)

**Horizontal Scaling (Scale Out)** – More machines

**Sharding** – Partition data across nodes by key

**Replication** – Copy data to multiple nodes

**Leader-Follower** – One write node (leader), many read nodes (followers)

**Multi-Master** – Multiple nodes accept writes

---

## Performance Metrics

**Latency** – Time to process single request (milliseconds)

**Throughput** – Requests per second

**Bandwidth** – Data transfer rate (MB/s, GB/s)

**QPS (Queries Per Second)** – Read throughput metric

**TPS (Transactions Per Second)** – Write throughput metric

**P50, P95, P99** – Percentile latencies (50th, 95th, 99th)

---

## Reliability Metrics

**Availability** – Uptime percentage (99.9% = 43 min downtime/month)

**MTBF (Mean Time Between Failures)** – Average time system runs before failing

**MTTR (Mean Time To Recovery)** – Average time to restore after failure

**SLI (Service Level Indicator)** – Metric being measured (e.g., latency)

**SLO (Service Level Objective)** – Target for SLI (e.g., P99 < 200ms)

**SLA (Service Level Agreement)** – Legal contract with penalties

**RTO (Recovery Time Objective)** – Max acceptable downtime

**RPO (Recovery Point Objective)** – Max acceptable data loss

---

## Architecture Styles

**Monolith** – Single deployable application

**Microservices** – Independently deployable services

**Serverless** – Event-driven, auto-scaling functions (FaaS)

**Event-Driven** – Components communicate via events

**Service-Oriented (SOA)** – Modular services with shared enterprise bus

---

## Quick Lookup

| Need | Term | Why |
|------|------|-----|
| Prevent duplicate effects | Idempotency | Safe retries |
| Overloaded partition | Hot partition | Rebalance shards |
| Server affinity issue | Sticky session | Use stateless design |
| Slow first request | Cold start | Warm up or cache |
| Simultaneous retries | Thundering herd | Add jitter to backoff |
| Multiple leaders | Split brain | Use consensus algorithm |

---

## Acronyms Decoded

- **ACID** – Atomicity, Consistency, Isolation, Durability
- **BASE** – Basically Available, Soft state, Eventual consistency
- **CAP** – Consistency, Availability, Partition tolerance
- **CQRS** – Command Query Responsibility Segregation
- **CDN** – Content Delivery Network
- **DNS** – Domain Name System
- **gRPC** – Google Remote Procedure Call
- **JWT** – JSON Web Token
- **RBAC** – Role-Based Access Control
- **REST** – Representational State Transfer
- **RPC** – Remote Procedure Call
- **TTL** – Time To Live

