# System Design Basics

A structured guide to fundamental system design concepts. Each folder contains focused, practical content on specific topics.

---

## Module Structure

### 1. [Fundamentals](./fundamentals)
Core concepts and terminology
- Scaling strategies (vertical vs horizontal)
- Latency vs throughput
- SLA, SLO, SLI

### 2. [Architecture](./architecture)
System properties and trade-offs
- Availability, reliability, fault tolerance
- Maintainability and cost considerations
- Design trade-offs

### 3. [Data & Storage](./data-storage)
Database and storage patterns
- RDBMS vs NoSQL
- Sharding and replication
- Indexing strategies

### 4. [Networking](./networking)
Distribution and communication
- Load balancing and CDNs
- Protocols (HTTP/2, gRPC, TCP/UDP)
- Reverse proxies and API gateways

### 5. [Reliability & Consistency](./reliability)
Distributed system guarantees
- CAP theorem and PACELC
- Consistency models
- Consensus algorithms

### 6. [Performance](./performance)
Optimization techniques
- Caching strategies
- Rate limiting and throttling
- Backpressure and async processing

### 7. [Building Blocks](./building-blocks)
Common patterns and practices
- Microservices vs monolith
- Circuit breakers and retries
- Event sourcing and CQRS

### 8. [Operations](./operations)
Production concerns
- Monitoring and observability
- Deployment strategies
- Security and disaster recovery

---

## Quick Start Guide

**For interviews:** Focus on fundamentals, architecture, and reliability first.  
**For implementation:** Study building-blocks and operations.  
**For optimization:** Deep dive into performance and data-storage.

---

## Design Checklist

1. ✓ Clarify requirements (scale, latency, budget)
2. ✓ Sketch core flows and data model
3. ✓ Choose storage and justify
4. ✓ Identify bottlenecks and scaling strategies
5. ✓ Plan for availability and consistency
6. ✓ Add monitoring and security
7. ✓ Highlight trade-offs

---

## References

- *Designing Data-Intensive Applications* by Martin Kleppmann
- *System Design Interview* by Alex Xu
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [Google SRE Book](https://sre.google/books/)

