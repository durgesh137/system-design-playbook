# Networking & Distribution

Network components and protocols for distributed systems.

---

## CDN (Content Delivery Network)

Geographically distributed cache servers for static assets.

**How it works:**
1. User requests asset (image, video, JS)
2. CDN edge server near user serves cached copy
3. If cache miss, fetch from origin and cache

**Benefits:**
- Reduced latency (geographic proximity)
- Lower origin server load
- DDoS protection

**Use for:** Images, videos, CSS/JS, downloads

**Examples:** CloudFlare, Akamai, AWS CloudFront, Fastly

---

## Load Balancer

Distributes incoming traffic across multiple servers.

### Layer 4 (Transport Layer)
Routes based on IP and TCP/UDP port.
- **Fast:** Simple packet forwarding
- **Limited:** No HTTP-level routing

### Layer 7 (Application Layer)
Routes based on HTTP headers, URL, cookies.
- **Flexible:** Path-based routing, SSL termination
- **Slower:** Deeper packet inspection

**Algorithms:**
- **Round Robin:** Distribute evenly
- **Least Connections:** Send to least busy server
- **IP Hash:** Same client → same server (sticky sessions)

**Examples:** Nginx, HAProxy, AWS ELB/ALB, Google Cloud Load Balancing

---

## Reverse Proxy

Server between clients and backend services.

**Functions:**
- Route requests to appropriate backend
- SSL/TLS termination (decrypt HTTPS)
- Caching static content
- Compression and rate limiting
- Hide backend topology

**Examples:** Nginx, Apache, Envoy

---

## Protocols

### TCP vs UDP

| TCP | UDP |
|-----|-----|
| Reliable, ordered | Fast, connectionless |
| Connection setup (handshake) | No handshake |
| Error recovery | No guarantees |
| HTTP, database connections | Video streaming, DNS, gaming |

### HTTP/1.1 vs HTTP/2 vs HTTP/3

**HTTP/1.1:**
- One request per TCP connection (or serial on persistent connection)
- Head-of-line blocking

**HTTP/2:**
- Multiplexing (multiple streams on one connection)
- Server push, header compression
- Still over TCP

**HTTP/3:**
- Uses QUIC (UDP-based)
- Faster handshake, better mobile performance

### gRPC

Binary protocol over HTTP/2 for microservices.
- **Pros:** Fast serialization (Protocol Buffers), streaming support
- **Cons:** Less human-readable than REST/JSON
- **Use for:** Service-to-service communication

---

## API Gateway

Single entry point for client applications.

**Responsibilities:**
- Authentication and authorization
- Request routing to microservices
- Rate limiting and throttling
- Protocol translation (REST → gRPC)
- Response aggregation

**Examples:** Kong, AWS API Gateway, Apigee, Tyk

---

## Network Patterns

### Service Mesh
Infrastructure layer for service-to-service communication.
- **Examples:** Istio, Linkerd, Consul
- **Features:** Traffic management, observability, security

### DNS
Translates domain names to IP addresses.
- **Use for:** Service discovery, load distribution, failover
- **Example:** Route53, CloudFlare DNS

---

## Quick Decision Guide

| Scenario | Solution | Why |
|----------|----------|-----|
| Static assets | CDN | Reduce latency |
| Multiple backends | Load balancer | Distribute traffic |
| SSL termination | Reverse proxy | Centralized certificate management |
| Real-time data | UDP/WebSocket | Low latency |
| Microservices | gRPC | Fast, typed contracts |
| Client API | API Gateway | Unified entry point |

---

## Performance Tips

- **Use CDN** for all static content
- **Enable HTTP/2** for multiplexing benefits
- **Connection pooling** to avoid handshake overhead
- **Compression** (gzip, brotli) for text responses
- **DNS caching** to reduce lookup time

---

## References

- [High Performance Browser Networking](https://hpbn.co/) by Ilya Grigorik
- [Nginx Architecture](https://www.nginx.com/blog/inside-nginx-how-we-designed-for-performance-scale/)
- [AWS CDN Best Practices](https://aws.amazon.com/cloudfront/getting-started/)

