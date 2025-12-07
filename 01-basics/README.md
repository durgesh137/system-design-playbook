# System Design Basics

## Core Concepts

- **Scalability** – System's ability to handle growth in users, data, or requests
- **Availability** – Percentage of time a system remains operational and accessible
- **Latency** – Time delay between request initiation and response delivery
- **Throughput** – Number of operations or requests processed per unit of time
- **Consistency** – Guarantee that all nodes see the same data at the same time
- **Partition Tolerance** – System continues operating despite network failures
- **Load Balancing** – Distributing incoming traffic across multiple servers
- **Caching** – Storing frequently accessed data in fast-access storage layers
- **Replication** – Copying data across multiple nodes for redundancy and performance
- **Sharding** – Horizontally partitioning data across multiple databases

## When to Use

- Start with vertical scaling (upgrade resources) for simplicity; move to horizontal scaling (add servers) when limits hit
- Choose strong consistency for financial/critical data; eventual consistency for social feeds, analytics, or high-scale reads

## References

- *Designing Data-Intensive Applications* by Martin Kleppmann
- *System Design Interview* by Alex Xu
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [Google SRE Book](https://sre.google/books/)

