# Everyday Backend Patterns

Concise problem cards for common backend operations.

---

## Authentication & Authorization

Verify identity (authn) and control access (authz).

- **Constraints:** Token expiry, secret rotation, session storage limits
- **Key Components:** Identity provider, JWT/sessions, role-based access control (RBAC)
- **Trade-offs:** Stateless tokens (scalable but harder to revoke) vs. stateful sessions (easy revocation but requires shared storage)
- **Failure Mode:** Token leakage or replay attacks if not using HTTPS/short expiry
- **Monitoring Metric:** Failed login attempts per minute

---

## Signup/Password Reset

User registration and credential recovery flows.

- **Constraints:** Email delivery delays, token expiration windows (15-60 min)
- **Key Components:** Email service, one-time tokens, rate limiting on requests
- **Trade-offs:** Security (complex tokens, short expiry) vs. UX (simple flows, longer windows)
- **Failure Mode:** Email not delivered or caught in spam filters
- **Monitoring Metric:** Password reset success rate

---

## File & Media Uploads

Handle user-uploaded images, videos, documents.

- **Constraints:** File size limits, allowed MIME types, storage quotas
- **Key Components:** Object storage (S3), CDN, virus scanning, thumbnail generation
- **Trade-offs:** Direct uploads (fast, less server load) vs. proxied uploads (easier validation, more control)
- **Failure Mode:** Large files timing out or filling disk if not streamed
- **Monitoring Metric:** Upload failure rate by file size

---

## Background Jobs & Queues

Offload long-running or deferred tasks.

- **Constraints:** Queue depth limits, job timeout settings, retry budgets
- **Key Components:** Message broker (RabbitMQ, SQS), worker processes, dead-letter queue
- **Trade-offs:** At-most-once (fast, may lose jobs) vs. at-least-once (reliable but may duplicate)
- **Failure Mode:** Worker crashes mid-job without ack/rollback
- **Monitoring Metric:** Queue lag (messages waiting)

---

## Notifications & Email

Send transactional or promotional messages to users.

- **Constraints:** Rate limits from providers, deliverability reputation, latency for time-sensitive alerts
- **Key Components:** Email/SMS provider (SendGrid, Twilio), template engine, opt-out management
- **Trade-offs:** Real-time push (low latency) vs. batched sends (higher throughput, lower cost)
- **Failure Mode:** Provider outage or blacklisting due to spam complaints
- **Monitoring Metric:** Delivery success rate

---

## Search & Pagination

Enable users to find and browse data efficiently.

- **Constraints:** Index size, query complexity, result relevance tuning
- **Key Components:** Search engine (Elasticsearch, Algolia), cursor or offset pagination, filters/facets
- **Trade-offs:** Offset pagination (simple, allows jumping pages) vs. cursor pagination (efficient for deep pages, no skipped results)
- **Failure Mode:** Stale index or query timeout on complex filters
- **Monitoring Metric:** P95 search query latency

---

## Rate Limiting & Throttling

Protect APIs from abuse and overload.

- **Constraints:** Window size (fixed, sliding), per-user vs. global limits
- **Key Components:** Token bucket or leaky bucket algorithm, distributed counter (Redis)
- **Trade-offs:** Strict limits (predictable load) vs. burst allowance (better UX for spiky traffic)
- **Failure Mode:** Counter state lost if cache evicts or crashes
- **Monitoring Metric:** Requests rejected per second

---

## Feature Flags

Toggle features on/off without redeployment.

- **Constraints:** Flag evaluation latency, rollout percentage granularity, flag cleanup debt
- **Key Components:** Flag service (LaunchDarkly, in-house), targeting rules, logging/analytics
- **Trade-offs:** Runtime flexibility vs. code complexity and potential for stale flags
- **Failure Mode:** Flag service unavailable—defaults to safe fallback or cached state
- **Monitoring Metric:** Flag evaluation errors

---

## References

- [The Twelve-Factor App](https://12factor.net/)
- *Release It!* by Michael Nygard
- [AWS Architecture Blog](https://aws.amazon.com/blogs/architecture/)
- [High Scalability Blog](http://highscalability.com/)

