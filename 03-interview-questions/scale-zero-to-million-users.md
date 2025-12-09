# Scaling Web Applications: Zero to One Million Users

> **Problem Statement:** How to scale a web application from zero to one million users?

---

## Step 0: Single Server Request-Response Flow

**Basic flow of a single request in a single server setup:**

- **User** => sends request
- **Server** => receives request and processes it
- **Database** => stores and retrieves data
- **Server** => sends response
- **User** => receives response

---

## Phase 1: Understanding the Basics

### Step 1.1: Request Flow in Web Applications

**Flow:**
1. User => enters `dosomething.com` on web browser => resulting in **DNS resolution** => request sent to web server
2. This step is termed as DNS resolution
3. This request goes to DNS server to resolve the domain name to IP address
4. DNS server => responds with IP address of web server
5. User => browser sends HTTP request to web server at that IP address
6. Further request is sent to web server

**Server-side processing:**
- Now server side logic comes into picture, to process the request
- Different programming languages like Java, JavaScript, Python, Ruby, PHP, etc can be used to write server side logic
- This is done to perform the business logic of the application

**Response:**
- Further server gives response either in form of HTML or JSON or other formats
- Then finally the response is rendered on user browser

### Step 1.2: Request Received from Mobile App

Here request is sent from mobile app to web server via internet:

- **Mobile app** => sends HTTP request to web server
- **Web server** => processes request and interacts with database if needed
- **Web server** => sends response back to mobile app
- **Mobile app** => receives response and displays data to user

**Response format:** JSON, XML, or other formats

---

## Phase 2: Scaling for Increased Traffic (1000 requests/second)

### Step 2.0: How to Handle Increased Traffic?

**Challenge:** Here single server won't be able to handle lot of requests, how can we handle this situation?

### Step 2.1: Deciding How to Scale?

#### Option A: Vertical Scaling (Increase Capacity of Single Server)

Yes lets try that.

**How much can we increase capacity of single server?**
- **Max CPU possible:** 128 cores (theoretical limit)
- **Max RAM possible:** 512 GB (theoretical limit)

**Limitations:**

Overall, there is a limit till which we can increase capacity of single server.

**Risk - Single Point of Failure (SPOF):**

In case besides increase all the capacity of single server, if this server goes down:
- It will just stop the entire application, hence not a single user will be able to access it
- This is called **single point of failure (SPOF)**

#### Option B: Horizontal Scaling

**But still single server has its limits, what if traffic increases more?**

Is there any other alternate?

Can we have replica of existing servers, like if we can add more servers to handle more traffic?

**Answer:** Yes we can do that, this is called **horizontal scaling**

### Step 2.2: Horizontal Scaling

**Two Action Items:**
1. First our application should not go down in case of server failure
2. How to ensure excessive traffic is handled properly?

**Implementation:**

We can add multiple servers to handle more traffic.

**Challenge:** Say we have added 2 servers to handle traffic, how to ensure all servers are utilized properly?

Since our goal is to prevent single point of failure. What if all requests are still going to single server?

**How to find out?** There should be mechanism to route the requests to different servers.

Here comes the concept of **Load Balancer**

### Step 2.3: Load Balancer
Load balancer is a server that sits between user and web servers.
**Functionality:**
- It receives all incoming requests from users
- It routes these requests to different web servers based on certain algorithms
- This ensures that no single server is overwhelmed with too many requests

Normally the public IP address of load balancer is given to users, so that all requests first go to load balancer.
The IP address of web servers are hidden from public, termed as private IP addresses.
- These private IP addresses are only accessible between the servers within the network.

Load Balancers communicates with web servers using these private IP addresses.

**Common Load Balancing Algorithms:**
1. **Round Robin:** Distributes requests evenly across all servers in a circular order.
2. **Least Connections:** Directs traffic to the server with the fewest active connections.
3. **IP Hash:** Routes requests based on the client's IP address, ensuring consistent routing for the same client.
4. **Weighted Round Robin:** Assigns weights to servers based on their capacity, directing more traffic to higher-capacity servers.
5. **Random:** Distributes requests randomly across all servers.
6. **Least Response Time:** Sends requests to the server with the lowest response time.
7. **Geographic Load Balancing:** Routes traffic based on the geographic location of the user to the nearest server.
8. **Health Check-Based:** Monitors server health and routes traffic only to healthy servers.

### Step 2.4 : What if both the servers went down? How to resolve that?
We can find server status using load balancers only.
**How does load balancer know if server is down?**
- Load balancer periodically sends health check requests to web servers
- If server does not respond within certain time, it is marked as down
- Load balancer stops sending requests to that server until it is back up

**Right now we have 2 servers, what if both servers went down?**
- In that case load balancer will not be able to route requests to any server
- To resolve this, we can have few servers in backup, in case some server is down, can we direct load balancer to auto-detect and add new servers

We can use auto-scaling groups for this purpose.
- Configure auto-scaling groups to automatically add or remove servers based on traffic load and server health.

---

## Phase 3: Database Considerations and Scaling

### Step 3.0: Choosing the Right Database Type

Now that we have scaled our application servers, the database often becomes the next bottleneck. Before we scale the database, we need to decide which type of database fits our needs.

#### Relational Databases (SQL)

**Examples:** MySQL, PostgreSQL, Oracle, SQL Server

**When to use:**
- Data has clear relationships and structure
- Need ACID transactions (Atomicity, Consistency, Isolation, Durability)
- Complex queries with JOINs are required
- Data consistency is critical (e.g., financial transactions, order management)
- Structured data with predefined schema

**Advantages:**
- Strong consistency guarantees
- Mature ecosystem with excellent tooling
- Complex query support with SQL
- ACID compliance ensures data integrity

**Disadvantages:**
- Harder to scale horizontally
- Schema changes can be complex
- Performance can degrade with very large datasets

#### NoSQL Databases

**Examples:** MongoDB (Document), Cassandra (Wide-column), Redis (Key-value), Neo4j (Graph)

**When to use:**
- Massive scale and high throughput needed
- Flexible or evolving schema requirements
- Unstructured or semi-structured data
- Horizontal scaling is a priority
- Specific data models (documents, graphs, key-value pairs)

**Advantages:**
- Easy horizontal scaling
- Flexible schema
- High performance for specific use cases
- Better for handling large volumes of data

**Disadvantages:**
- Eventual consistency (in many cases)
- Limited support for complex queries and JOINs
- Different query languages per database type
- May sacrifice consistency for availability (CAP theorem)

#### Decision Framework

**Choose Relational (SQL) if:**
- Your data is structured and relational
- You need strong consistency and ACID transactions
- Complex queries and reporting are important
- Your team is familiar with SQL

**Choose NoSQL if:**
- You need to scale horizontally across many servers
- Your data model is flexible or constantly evolving
- You prioritize availability and partition tolerance over consistency
- You have specific use cases (caching, real-time analytics, etc.)

**Hybrid Approach:**
Many modern applications use both:
- Relational database for core transactional data
- NoSQL for caching, session storage, or specific high-volume features

### Step 3.1: Database Scaling Strategies
Once the database type is chosen, we need to consider scaling strategies to handle increased load.
#### Vertical Scaling
- Increase the resources (CPU, RAM, SSD) of the existing database server.
- Simple to implement but has limits and can lead to SPOF.
- Best for small to medium workloads.

#### Horizontal Scaling
Similar to application servers, can we maintain multiple database servers?
Possibly yes, but how to maintain data consistency, since it will involve CRUD operations and more complex queries, how will we keep all databases in sync?
- this technique of maintaining multiple database servers is called **Database Replication**

### Step 3.2: How Database Replication Works?
**Types of Replication:**
1. **Master-Slave Replication:**
   - One primary (master) database handles write operations.
   - Multiple secondary (slave) databases handle read operations.
   - Changes from the master are asynchronously replicated to the slaves.
   - Useful for read-heavy workloads.
   - Drawback: Write operations can become a bottleneck.

**Architecture:**
```
                        Write Operations (INSERT, UPDATE, DELETE)
                                      ↓
                         ┌─────────────────────────┐
                         │   Master DB (gdsDev)    │
                         │   IP: 10.0.1.10:3306    │
                         └─────────────────────────┘
                                      │
                    ┌─────────────────┼─────────────────┐
                    │  Replication    │  Replication    │
                    ↓  (Binary Log)   ↓  (Binary Log)   ↓
         ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐
         │ Slave 1 (Read)   │  │ Slave 2 (Read)   │  │ Slave 3 (Read)   │
         │ IP: 10.0.1.11    │  │ IP: 10.0.1.12    │  │ IP: 10.0.1.13    │
         └──────────────────┘  └──────────────────┘  └──────────────────┘
                    ↑                 ↑                 ↑
                    └─────────────────┴─────────────────┘
                         Read Operations (SELECT)
```


**Core Techniques:**
- **Read Replicas:** Distribute read traffic across multiple database copies
- **Sharding:** Split database into smaller pieces based on shard key (horizontal partitioning)
- **Database Clustering:** Multiple servers working as single system for high availability
- **Caching:** Use Redis/Memcached to reduce database load for frequently accessed data
- **Connection Pooling:** Reuse database connections to reduce overhead

**Supporting Strategies:**
- **Load Balancing:** Distribute requests across multiple database servers
- **Backup and Recovery:** Robust strategies to prevent data loss
- **Monitoring and Optimization:** Continuously monitor performance and optimize queries
- **CDN:** Offload static content from database
- **Database as a Service (DBaaS):** Managed cloud database services with built-in scaling

**Maintenance and Best Practices:**
- **Data Archiving:** Archive old data to improve performance
- **Data Partitioning:** Organize data into partitions for better query performance
- **Regular Maintenance:** Indexing, vacuuming, analyzing databases
- **Security Measures:** Protect against unauthorized access
- **Disaster Recovery Planning:** Ensure business continuity


---

## Terminology:
- **Vertical Scaling:** Increasing the capacity of a single server (CPU, RAM, etc.)
- **Horizontal Scaling:** Adding more servers to handle increased traffic
- **Load Balancer:** A server that distributes incoming requests across multiple servers to ensure no single server is overwhelmed
- **Single Point of Failure (SPOF):** A component whose failure would cause the entire system to fail
- **Health Check:** A mechanism used by load balancers to monitor the status of servers and ensure they are operational
- **Active-Passive Configuration:** A setup where one load balancer is active and the other is on standby, ready to take over if the active one fails
- **Active-Active Configuration:** A setup where both load balancers are active and share the load, providing redundancy and high availability