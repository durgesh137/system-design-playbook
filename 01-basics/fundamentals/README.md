# Fundamentals

Core concepts that form the foundation of system design.

---

## Scaling Strategies

### Vertical Scaling (Scale Up)
Increase resources (CPU, RAM, disk) of a single machine.
- **Pros:** Simple, no code changes, strong consistency
- **Cons:** Hardware limits, single point of failure, expensive at high end
- **When:** Early stages, databases requiring ACID, low-medium traffic

### Horizontal Scaling (Scale Out)
Add more machines to distribute load.
- **Pros:** No theoretical limit, fault tolerance, cost-effective
- **Cons:** Complexity (load balancing, data partitioning), eventual consistency challenges
- **When:** High traffic, need redundancy, web/app servers

---

## Latency vs Throughput

**Latency:** Time to process a single request (milliseconds)
- **Low latency:** Real-time gaming, trading systems, video calls
- **Example:** API responds in 50ms

**Throughput:** Number of requests processed per unit time (requests/sec)
- **High throughput:** Batch processing, analytics, streaming
- **Example:** System handles 10,000 req/s

**Trade-off:** Optimizing for low latency (immediate response) may reduce throughput (fewer concurrent requests).

---

## SLA, SLO, SLI

### SLI (Service Level Indicator)
Measurable metric of service behavior.
- **Examples:** P99 latency, error rate, uptime percentage

### SLO (Service Level Objective)
Target value for an SLI.
- **Examples:** "P99 latency < 200ms", "99.9% uptime"

### SLA (Service Level Agreement)
Business contract with penalties for missing SLOs.
- **Examples:** "99.95% uptime or customer gets credit"

**Hierarchy:** SLI (what we measure) → SLO (our goal) → SLA (legal promise)

---

## System Components

**System:** Collection of services, databases, networks working together.

**Requirements:**
- **Functional:** What the system must do (features, APIs)
- **Non-functional:** How well it must perform (latency, throughput, availability, cost)

---

## Quick Reference

| Concept | Definition | Example |
|---------|-----------|---------|
| Scale | Handle growth | 100 → 1M users |
| Latency | Response time | 50ms API call |
| Throughput | Ops/time | 5000 req/s |
| SLO | Target metric | 99.9% uptime |

---

## References

- [Google SRE: SLIs, SLOs, SLAs](https://sre.google/sre-book/service-level-objectives/)
- *System Design Interview* by Alex Xu (Chapter 1)

