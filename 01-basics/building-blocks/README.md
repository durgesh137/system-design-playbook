# Building Blocks & Patterns

Common architectural patterns for distributed systems.

---

## Microservices vs Monolith

### Monolith
Single deployable unit containing all functionality.

**Pros:**
- Simple to develop and deploy initially
- Easy to test (single process)
- Strong consistency (shared DB)
- Lower operational overhead

**Cons:**
- Tight coupling (changes affect everything)
- Difficult to scale (all-or-nothing)
- Deployment risk (one bug breaks all)
- Technology lock-in

### Microservices
Application split into independently deployable services.

**Pros:**
- Independent scaling and deployment
- Technology flexibility per service
- Fault isolation
- Team autonomy

**Cons:**
- Distributed system complexity
- Network overhead and latency
- Data consistency challenges
- More operational burden

**When to use Microservices:**
- Large team (>10-15 engineers)
- Different scaling needs per feature
- Need for independent deployments

---

## API Gateway

Single entry point for all client requests.

**Responsibilities:**
- Authentication and authorization
- Request routing to services
- Rate limiting and throttling
- Response aggregation (one call → multiple services)
- Protocol translation (REST → gRPC)
- Caching and logging

**Examples:** Kong, AWS API Gateway, Apigee, Tyk

---

## Circuit Breaker

Stop calling failing downstream services to prevent cascade failures.

**States:**
1. **Closed:** Normal operation, requests pass through
2. **Open:** Service failing, reject requests immediately
3. **Half-Open:** Test if service recovered

**Thresholds:**
- Open circuit after N failures in M seconds
- Stay open for timeout period
- Test with limited requests in half-open

**Benefits:** Fast failure, reduce load on struggling service, give time to recover

**Implementation:** Hystrix, Resilience4j, Polly

---

## Retry with Backoff

Handle transient failures safely.

**Exponential Backoff:**
- Wait 1s, 2s, 4s, 8s between retries
- Prevents thundering herd

**Jitter:**
- Add randomness to backoff (avoid synchronized retries)
- Example: Wait 1s ± random(0-200ms)

**Idempotency:**
- Ensure retries don't duplicate effects
- Use request IDs or natural keys

**Max Retries:**
- Give up after N attempts (avoid infinite loops)

---

## Event Sourcing

Store state changes as sequence of events instead of current state.

**Benefits:**
- Complete audit trail
- Time travel (replay events to any point)
- Debug and analytics from event log

**Challenges:**
- Event schema evolution
- Query complexity (need to replay)
- Storage growth

**Use for:** Financial ledgers, audit logs, collaborative systems

---

## CQRS (Command Query Responsibility Segregation)

Separate read and write models.

**Write Model (Commands):**
- Optimized for updates
- Enforce business rules
- Normalized schema

**Read Model (Queries):**
- Optimized for reads
- Denormalized views
- Can have multiple read models for different use cases

**Benefits:** Scale reads and writes independently, optimize each side

**Complexity:** Data synchronization, eventual consistency between models

---

## Saga Pattern

Manage distributed transactions across microservices.

**Problem:** No ACID transactions across services

**Solution:** Break transaction into local transactions with compensating actions.

### Choreography
Services publish events; others react.
- **Pros:** Loose coupling
- **Cons:** Hard to track progress

### Orchestration
Central coordinator manages saga.
- **Pros:** Centralized logic, easier to monitor
- **Cons:** Single point of coordination

**Use for:** E-commerce checkout, booking workflows

---

## Bulkhead Pattern

Isolate resources to prevent failures from spreading.

**Example:** Separate thread pools per service dependency
- Service A failure doesn't exhaust threads for Service B

**Like:** Ship compartments that prevent total sinking

---

## Service Mesh

Infrastructure layer for service-to-service communication.

**Features:**
- Traffic management (routing, load balancing)
- Security (mTLS, authentication)
- Observability (metrics, tracing)
- Resilience (retries, circuit breakers)

**Examples:** Istio, Linkerd, Consul Connect

**Trade-off:** Powerful features vs operational complexity

---

## Quick Decision Guide

| Pattern | When to Use | Example |
|---------|-------------|---------|
| Monolith | Small team, simple domain | MVP, early startup |
| Microservices | Large team, complex domain | Netflix, Amazon |
| API Gateway | Multiple clients, auth needs | Mobile + web app |
| Circuit Breaker | Depend on flaky services | Third-party APIs |
| Event Sourcing | Need audit trail | Financial systems |
| CQRS | Read/write patterns differ | Analytics dashboard |
| Saga | Distributed transactions | Order + payment + shipping |

---

## Anti-Patterns to Avoid

- **Distributed monolith:** Microservices sharing database
- **Chatty services:** Excessive inter-service calls
- **Sync calls for everything:** Use async/events when appropriate
- **No circuit breakers:** Cascading failures
- **Premature microservices:** Start with monolith

---

## References

- *Building Microservices* by Sam Newman
- [Microservices.io Patterns](https://microservices.io/patterns/)
- [Release It!](https://pragprog.com/titles/mnee2/release-it-second-edition/) by Michael Nygard
- [Martin Fowler's Blog](https://martinfowler.com/microservices/)

