# Performance Optimization

Techniques to improve system speed, efficiency, and resource utilization.

---

## Caching

Store frequently accessed data in fast-access storage.

### Cache Levels
1. **Client-side:** Browser cache, app memory
2. **CDN:** Edge servers for static assets
3. **Application:** Redis, Memcached (in-memory)
4. **Database:** Query cache, buffer pool

### Strategies

**Cache-Aside (Lazy Loading)**
1. Check cache
2. If miss, fetch from DB and populate cache
- **Pros:** Only cache requested data
- **Cons:** Initial request is slow

**Write-Through**
Write to cache and DB simultaneously.
- **Pros:** Cache always fresh
- **Cons:** Slower writes

**Write-Back (Write-Behind)**
Write to cache, async write to DB.
- **Pros:** Fast writes
- **Cons:** Data loss risk if cache fails

### Invalidation
"There are only two hard things in Computer Science: cache invalidation and naming things."
- **TTL (Time To Live):** Expire after duration
- **Event-based:** Invalidate on updates
- **LRU (Least Recently Used):** Evict old entries

---

## Rate Limiting & Throttling

Protect systems from overload and abuse.

### Algorithms

**Token Bucket**
- Bucket holds tokens; each request consumes one
- Refill at fixed rate
- **Allows bursts** up to bucket size

**Leaky Bucket**
- Requests enter bucket, processed at fixed rate
- **Smooth traffic**, no bursts

**Fixed Window**
- Count requests in time window (e.g., 100/min)
- **Simple** but allows double burst at boundaries

**Sliding Window Log**
- Track timestamp of each request
- **Accurate** but memory-intensive

### Implementation
**Distributed:** Use Redis with atomic operations  
**Per-service:** In-memory counters

---

## Batch Processing

Group operations to amortize overhead.

**Benefits:**
- Reduce network round trips
- Amortize connection/transaction cost
- Higher throughput

**Examples:**
- Bulk database inserts
- Batch ETL jobs
- Email send batches

**Trade-off:** Latency (wait for batch) vs efficiency

---

## Indexing & Denormalization

### Database Indexing
Speed up reads at cost of writes and storage.
- **When:** Frequent queries on specific columns
- **Watch:** Index maintenance overhead

### Denormalization
Store redundant data to avoid JOINs.
- **Example:** Store username with each post (avoid JOIN with users table)
- **Trade-off:** Faster reads, slower writes, data sync complexity

---

## Async Processing & Queues

Decouple producers and consumers.

**Benefits:**
- Handle traffic spikes (queue buffers)
- Retry failed operations
- Scale producers/consumers independently

**Patterns:**
- **Fire-and-forget:** Send message, don't wait
- **Request-reply:** Async request with callback
- **Pub-Sub:** Multiple consumers for one message

**Examples:** RabbitMQ, Kafka, AWS SQS, Redis Streams

---

## Backpressure

Signal senders to slow down when consumers are overloaded.

**Strategies:**
- Reject requests with 429 (Too Many Requests)
- Apply rate limits
- Use bounded queues (block when full)
- Circuit breaker pattern

---

## Connection Pooling

Reuse database/service connections instead of creating new ones.

**Benefits:**
- Avoid connection handshake overhead
- Limit concurrent connections to DB
- Better resource utilization

**Configuration:**
- Pool size (max connections)
- Timeout (max wait for connection)
- Keep-alive (prevent idle disconnect)

---

## Compression

Reduce data size for faster transfer.

**Algorithms:**
- **Gzip:** Good compression, moderate CPU
- **Brotli:** Better compression, more CPU
- **Snappy/LZ4:** Fast, less compression (internal use)

**Use for:**
- HTTP responses (text, JSON)
- Log shipping
- Database backups

**Trade-off:** CPU cost vs bandwidth savings

---

## Performance Checklist

- [ ] Cache frequently accessed data
- [ ] Add indexes for common queries
- [ ] Use CDN for static assets
- [ ] Implement rate limiting
- [ ] Enable compression
- [ ] Pool connections
- [ ] Batch operations where possible
- [ ] Monitor and profile before optimizing

---

## Quick Wins

| Problem | Solution | Impact |
|---------|----------|--------|
| Slow DB queries | Add indexes | 10-100x faster |
| High latency | Add caching layer | 100-1000x faster |
| Traffic spikes | Rate limiting + queues | Stability |
| Large responses | Compression | 60-80% reduction |
| Many DB connections | Connection pooling | Better utilization |

---

## References

- *High Performance Browser Networking* by Ilya Grigorik
- [Redis Best Practices](https://redis.io/topics/optimization)
- [Caching Strategies](https://aws.amazon.com/caching/best-practices/)
- [Rate Limiting Algorithms](https://stripe.com/blog/rate-limiters)

