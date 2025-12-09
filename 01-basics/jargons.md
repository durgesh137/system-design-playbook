# System Design — Jargon & Fundamentals

This file collects concise, practical definitions and core concepts that appear often during system design interviews and real-world architecture work. Use this as a living reference and expand examples or diagrams per topic.

---

## Table of contents
- Fundamentals
- Architecture properties & trade-offs
- Data & storage
- Networking & distribution
- Reliability & consistency
- Performance optimisation
- Common building blocks & patterns
- Operational concerns
- Short checklist for designs
- Further reading / exercises

---

## Fundamentals
- System: a collection of components (services, databases, networks) that work together to achieve goals.
- Requirement: explicit functional (what it must do) and non-functional requirements (latency, throughput, availability, cost).
- SLA / SLO / SLI:
    - SLI (Service Level Indicator): measured metric (e.g., p99 latency).
    - SLO (Service Level Objective): target for that metric (e.g., p99 < 200ms).
    - SLA (Service Level Agreement): legally binding promise, often tied to penalties.
- Scale: ability to handle growth (traffic, data).
    - Vertical scaling: bigger machine.
    - Horizontal scaling: add more machines.
- Latency vs Throughput:
    - Latency: time to respond to one request.
    - Throughput: requests served per unit time.

---

## Architecture properties & trade-offs
- Availability: % of time system is operational.
- Reliability: system operates without failure for expected duration.
- Fault tolerance: ability to continue functioning when parts fail.
- Consistency: degree to which clients see the same data at the same time.
- Maintainability: ease of updating and understanding.
- Cost: infrastructure and operational expenses.
- Trade-offs: improving one property often degrades others (e.g., stronger consistency can reduce availability).

---

## Data & storage
- RDBMS: relational database, ACID transactions, strong consistency.
- NoSQL: key-value, document, wide-column, or graph stores — often scaled horizontally and trade strong consistency for availability/partition tolerance.
- Sharding (partitioning): split data by key across nodes to scale writes and storage.
- Replication: copy data across nodes for read throughput and availability (master-slave, multi-master).
- Indexing: data structures that speed read queries at the expense of write cost and storage.
- Time-series DB / OLAP / OLTP: databases optimized for specific access patterns.

---

## Networking & distribution
- CDN (Content Delivery Network): caches static assets near users to reduce latency.
- Load balancer: distributes traffic across servers (L4 vs L7).
- Reverse proxy: sits in front of services for routing, TLS termination, caching.
- TCP vs UDP: reliable ordered vs faster connectionless transport.
- HTTP/2, gRPC: multiplexing and lower overhead protocols for microservices.

---

## Reliability & consistency models
- CAP theorem: in presence of network Partition, a distributed system can be either Consistent or Available (but not both).
- PACELC: trade-off: Partition vs Availability, Else Latency vs Consistency.
- Strong consistency: all reads return latest write (e.g., single leader commits).
- Eventual consistency: updates propagate asynchronously; reads may be stale temporarily.
- Quorum systems: require majority (or configurable) subset to agree for operations.
- Consensus algorithms: Raft and Paxos for leader election and replicated state machines.

---

## Performance optimisation
- Caching: reduce latency and load (in-memory caches, CDNs). Watch cache invalidation and coherence.
- Batch processing: group operations to amortize overhead.
- Indexing and denormalization: speed reads; increase write/space cost.
- Rate limiting & throttling: protect systems from bursts and abuse.
- Backpressure: signal senders to slow down when consumers are overloaded.
- Async processing & message queues: decouple producers and consumers to handle spikes.

---

## Common building blocks & patterns
- Microservices vs Monolith: trade modularity vs operational complexity.
- API Gateway: single entry for clients; handles auth, routing, throttling.
- Circuit Breaker: stop calling failing downstream services to avoid cascades.
- Retry with exponential backoff + jitter: handle transient failures safely.
- Event sourcing & CQRS:
    - Event sourcing: persist state changes as events.
    - CQRS (Command Query Responsibility Segregation): separate read and write models.
- Saga pattern: manage distributed transactions with compensating actions.
- Bulkhead: isolate failures to prevent cross-impact.

---

## Operational concerns
- Monitoring & Observability: metrics (throughput, latency, error rates), logs, tracing (distributed tracing).
- Alerting: meaningful thresholds and runbooks.
- Deployments: canary, blue/green, rolling updates to reduce risk.
- Security: authentication, authorization, encryption (in transit & at rest), secrets management.
- Disaster recovery: backups, cross-region replication, RTO/RPO planning.

---

## Short checklist for designing a system
1. Clarify requirements and constraints (scale, latency, data retention, budget).
2. Sketch core read/write flows and data model.
3. Choose storage(s) and justify (RDBMS vs NoSQL, cold vs hot).
4. Identify bottlenecks and scaling strategies (caching, sharding, replication).
5. Plan for availability (redundancy, failover) and consistency (models).
6. Add operational concerns: monitoring, deployment, security.
7. Highlight trade-offs and alternatives.
8. Estimate capacity and costs; outline testing plans.

---

## Glossary (quick)
- Idempotency: safe to repeat operation without changing result.
- Hot partition / hotspot: partition receiving disproportionate traffic.
- Sticky session: user tied to same server; causes scaling constraints.
- Cold start: slow first-time initialization cost (e.g., serverless).
- Fan-out / fan-in: patterns where one message reaches many or many feed into one.
- Thundering herd: many clients retry simultaneously and overwhelm service.

---

## Exercises & examples to add
- Design a URL shortener (explain key selection, uniqueness, scaling).
- Design a social feed (read-heavy, denormalization, caching).
- Design a chat system (real-time messaging, message ordering).
- Add diagrams for each example (sequence + component diagram).

---

Further reading
- Patterns of Enterprise Application Architecture (Martin Fowler)
- Designing Data-Intensive Applications (Martin Kleppmann)
- Site Reliability Engineering (SRE books, Google)

---

If you want, I can:
- convert this into separate sub-files (e.g., caching.md, consistency.md),
- add diagrams and example solutions (URL shortener, feed),
- or create a short checklist template you can copy into new design notes.
