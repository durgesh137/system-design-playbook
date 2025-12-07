# Interview Questions

20 system design prompts with hints and trade-offs. Keep answers focused on architectural decisions, not implementation details.

---

## Easy

### 1. Design a URL Shortener

Create short aliases for long URLs and redirect users.

- Consider hash collisions and custom slugs
- Think about expiration policies
- Plan for analytics (click tracking)

**Trade-offs:** Random IDs (collision handling) vs. counter-based (predictable but needs distributed coordination).

---

### 2. Design a Pastebin

Allow users to upload and share text snippets.

- Handle storage for various snippet sizes
- Support expiration (auto-delete after TTL)
- Consider syntax highlighting metadata

**Trade-offs:** Store in DB (queryable) vs. object storage (cheaper for large blobs).

---

### 3. Design a Rate Limiter

Limit API requests per user or IP.

- Choose algorithm: token bucket, leaky bucket, fixed/sliding window
- Decide on distributed counter storage (Redis)
- Handle clock skew in distributed systems

**Trade-offs:** Strict enforcement (may frustrate users) vs. burst tolerance (better UX but less predictable load).

---

### 4. Design a Key-Value Store

Simple distributed storage for get/put operations.

- Decide on partitioning strategy (consistent hashing)
- Plan replication for availability
- Choose consistency model (strong vs. eventual)

**Trade-offs:** Strong consistency (slower writes, coordination overhead) vs. eventual consistency (higher throughput, stale reads possible).

---

### 5. Design a Web Crawler

Fetch and index web pages at scale.

- Implement politeness (respect robots.txt, rate limits per domain)
- Deduplicate URLs (visited set, Bloom filter)
- Handle link extraction and prioritization

**Trade-offs:** Breadth-first (discover wide) vs. prioritized crawl (focus on high-value pages first).

---

### 6. Design a Notification System

Send push, email, or SMS notifications to users.

- Support multiple channels and user preferences
- Queue messages for reliable delivery
- Handle retries and dead-letter scenarios

**Trade-offs:** Real-time delivery (low latency) vs. batching (cost-efficient, higher throughput).

---

### 7. Design a Metrics Dashboard

Display real-time system metrics (CPU, requests, errors).

- Aggregate time-series data efficiently
- Support drill-down and filtering
- Store raw vs. pre-aggregated data

**Trade-offs:** Raw data storage (flexible queries, high cost) vs. pre-aggregation (fast reads, limited flexibility).

---

## Medium

### 8. Design Instagram

Photo-sharing social network with feeds and likes.

- Handle image uploads, storage, and CDN delivery
- Build user feeds (fan-out on write vs. fan-out on read)
- Scale likes and comments

**Trade-offs:** Fan-out on write (fast reads, write amplification) vs. fan-out on read (slower reads, efficient writes).

---

### 9. Design Twitter

Microblogging platform with timelines and trends.

- Architect home timeline (followers' tweets)
- Handle high write volume and celebrity accounts
- Compute trending topics in near real-time

**Trade-offs:** Hybrid fan-out (pre-compute for most, on-demand for celebrities) balances write cost and read latency.

---

### 10. Design YouTube

Video upload, storage, streaming, and recommendations.

- Transcode videos into multiple formats/resolutions
- Use CDN for global distribution
- Build recommendation engine

**Trade-offs:** On-demand transcoding (saves storage, slower first view) vs. pre-transcode all (fast playback, high storage cost).

---

### 11. Design Uber

Ride-hailing with real-time driver matching.

- Geospatial indexing (quadtree, geohash)
- Match riders to nearby drivers
- Handle concurrent ride requests

**Trade-offs:** Exact location updates (accurate but high load) vs. coarse updates (reduced traffic, slight inaccuracy).

---

### 12. Design a Chat Application

Real-time messaging between users or groups.

- Choose WebSocket or long polling for live updates
- Store message history and support search
- Handle presence (online/offline status)

**Trade-offs:** Store all messages (full history, high cost) vs. retention policy (lower cost, lose old data).

---

### 13. Design a Booking System

Reserve seats, rooms, or appointments with no double-booking.

- Implement pessimistic or optimistic locking
- Handle concurrent booking attempts
- Support cancellations and refunds

**Trade-offs:** Pessimistic locking (guarantees no conflicts, lower concurrency) vs. optimistic locking (higher concurrency, retry on conflict).

---

### 14. Design a Search Autocomplete

Suggest queries as users type.

- Use trie or prefix tree for fast lookups
- Rank suggestions by popularity or relevance
- Update index with trending queries

**Trade-offs:** Real-time updates (always fresh) vs. periodic batch updates (lower load, slightly stale).

---

## Hard

### 15. Design Facebook News Feed

Personalized feed of posts from friends and pages.

- Rank posts by relevance (ML model or heuristic)
- Handle fan-out for millions of followers
- Support real-time updates and pagination

**Trade-offs:** Mixed fan-out (pre-compute for normal users, on-demand for high-follower accounts) optimizes cost and latency.

---

### 16. Design Netflix

Video streaming with personalized recommendations.

- Distribute content globally via CDN and edge caching
- Adaptive bitrate streaming
- Build recommendation engine with collaborative filtering

**Trade-offs:** Central catalog (consistent metadata) vs. regional replicas (lower latency, eventual consistency).

---

### 17. Design Amazon

E-commerce platform with product catalog, cart, and checkout.

- Handle inventory management and overselling
- Scale product search and filtering
- Process payments and order fulfillment

**Trade-offs:** Strong consistency for inventory (prevents overselling) vs. eventual consistency (higher availability, risk of temporary overselling).

---

### 18. Design Google Drive

Cloud storage with file sync and sharing.

- Sync files across devices (delta sync, conflict resolution)
- Store large files efficiently (chunking, deduplication)
- Manage permissions and sharing links

**Trade-offs:** Full file upload (simple) vs. block-level sync (efficient for large files, complex conflict handling).

---

### 19. Design a Distributed Cache

High-performance cache across multiple nodes.

- Use consistent hashing for key distribution
- Handle node failures and rebalancing
- Implement eviction policies (LRU, LFU)

**Trade-offs:** Replication (higher availability, more memory) vs. single copy (lower overhead, risk of data loss on failure).

---

### 20. Design a Stock Exchange

Match buy and sell orders with low latency.

- Maintain order book (priority queue, sorted by price/time)
- Handle high-frequency trades (microsecond latency)
- Ensure ACID properties for transactions

**Trade-offs:** In-memory matching engine (ultra-low latency) vs. disk-backed (durable but slower).

---

## References

- *System Design Interview* by Alex Xu (Volumes 1 & 2)
- [Grokking the System Design Interview](https://www.educative.io/courses/grokking-the-system-design-interview)
- [System Design Primer (GitHub)](https://github.com/donnemartin/system-design-primer)

