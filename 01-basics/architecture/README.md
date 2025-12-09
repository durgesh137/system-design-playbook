# Architecture Properties & Trade-offs

Understanding system qualities and the inherent compromises in design decisions.

---

## Core Properties

### Availability
Percentage of time system is operational and accessible.
- **Calculation:** (Uptime / Total Time) × 100
- **99.9% (3 nines):** ~43 min downtime/month
- **99.99% (4 nines):** ~4 min downtime/month
- **99.999% (5 nines):** ~26 sec downtime/month

**Improve with:** Redundancy, load balancing, health checks, auto-failover

---

### Reliability
System operates without failure for expected duration.
- **MTBF (Mean Time Between Failures):** Average time system runs before failing
- **MTTR (Mean Time To Recovery):** Average time to restore after failure

**Improve with:** Testing, graceful degradation, fault isolation

---

### Fault Tolerance
Ability to continue functioning when components fail.
- **Redundancy:** Duplicate critical components
- **Failover:** Automatic switch to backup
- **Graceful Degradation:** Reduce functionality instead of full failure

---

### Consistency
Degree to which all clients see the same data simultaneously.
- **Strong:** All reads see latest write (slower)
- **Eventual:** Updates propagate over time (faster)
- **Causal:** Preserves cause-effect order

---

### Maintainability
Ease of understanding, updating, and debugging the system.
- **Simplicity:** Minimize complexity
- **Modularity:** Loosely coupled components
- **Documentation:** Clear architecture diagrams and runbooks

---

### Cost
Infrastructure and operational expenses.
- **Fixed:** Servers, licenses, reserved capacity
- **Variable:** Traffic-based, storage, data transfer
- **Operational:** Engineering time, on-call, incidents

---

## The Trade-off Triangle

You rarely get all three simultaneously:

```
    Fast (Low Latency)
         /\
        /  \
       /    \
      /      \
     /________\
Cheap        Consistent
```

**Examples:**
- **Fast + Consistent = Expensive** (Strong consistency with low latency needs coordination overhead)
- **Fast + Cheap = Eventually Consistent** (Cache, CDN, eventual sync)
- **Cheap + Consistent = Slower** (Single database, no replication)

---

## Common Trade-offs

| Improve | Cost | Example |
|---------|------|---------|
| Availability | Consistency | Add replicas → stale reads |
| Consistency | Latency | Sync writes → slower |
| Performance | Cost | More servers, CDN |
| Simplicity | Features | Monolith vs microservices |
| Security | UX | MFA, rate limits |

---

## Design Philosophy

- **Start simple:** Premature optimization wastes time
- **Measure first:** Profile before optimizing
- **Know your constraints:** Budget, team size, deadline
- **Accept trade-offs:** Document and justify decisions

---

## References

- *The Pragmatic Programmer* (Hunt & Thomas)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- *Building Microservices* by Sam Newman

