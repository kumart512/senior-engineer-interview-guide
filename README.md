# 🚀 Senior Software Engineer Interview Preparation Guide
### *Google-Level — Coding, System Design & Software Architecture*

> **How to use this guide:** Work through each section methodically. Don't rush. Senior-level interviews test depth *and* breadth. Aim to spend 8–12 weeks if preparing from scratch.

---

## 📋 Table of Contents

1. [Interview Process Overview](#1-interview-process-overview)
2. [Mindset & Approach](#2-mindset--approach)
3. [Data Structures](#3-data-structures)
4. [Algorithms & Complexity](#4-algorithms--complexity)
5. [Coding Interview Patterns](#5-coding-interview-patterns)
6. [System Design Fundamentals](#6-system-design-fundamentals)
7. [System Design Deep Dives](#7-system-design-deep-dives)
8. [Software Architecture Patterns](#8-software-architecture-patterns)
9. [Databases & Storage](#9-databases--storage)
10. [Distributed Systems](#10-distributed-systems)
11. [Reliability & Observability](#11-reliability--observability)
12. [Security Fundamentals](#12-security-fundamentals)
13. [Leadership & Behavioral Questions](#13-leadership--behavioral-questions)
14. [Language-Specific Deep Dives](#14-language-specific-deep-dives)
15. [Practice Resources & Study Plan](#15-practice-resources--study-plan)

---

## 1. Interview Process Overview

### Typical Google-Level Senior Loop (5–6 rounds)
| Round | Focus | Duration |
|---|---|---|
| Phone Screen | Coding (1–2 problems) | 45 min |
| Coding Round 1 | Arrays, Strings, Trees | 45 min |
| Coding Round 2 | Graphs, DP, Advanced DS | 45 min |
| System Design | Scalable system design | 60 min |
| Behavioral / Leadership | Googleyness, leadership | 45 min |
| (Optional) Hiring Manager | Culture fit, vision | 30 min |

### What "Senior" Means to Interviewers
- You **drive** the conversation, not just answer questions.
- You identify **edge cases** without being prompted.
- You discuss **trade-offs** rather than single solutions.
- You demonstrate **ownership** of past projects.
- You show how you **influence** teams beyond individual coding.

---

## 2. Mindset & Approach

### The IDEO Framework for Coding
1. **I** — Identify constraints & edge cases (ask clarifying questions).
2. **D** — Design a brute-force solution first, state its complexity.
3. **E** — Explore optimizations (can we do better?).
4. **O** — Optimize and implement the best solution.

### Clarifying Questions Checklist
```
- What is the input size range?
- Can inputs be null/empty?
- Are there duplicate values?
- Is the input sorted?
- What should I return on invalid input?
- Can I modify the input in place?
- What's the memory budget?
```

### Communication Rules
- **Think out loud.** Silence is your enemy.
- Start with examples. Draw them out.
- State assumptions explicitly.
- Always analyze time AND space complexity.
- Review your code for bugs before saying "done."

---

## 3. Data Structures

### Arrays & Strings
- **Access:** O(1) | **Search:** O(n) | **Insert/Delete at end:** O(1) amortized
- Know: Two pointers, sliding window, prefix sums, in-place manipulation.
- Common pitfalls: off-by-one errors, integer overflow, Unicode edge cases in strings.

### Linked Lists
- Singly vs. doubly linked.
- **Floyd's Cycle Detection** (fast/slow pointer).
- Reversal, merging sorted lists, finding middle node.
- Sentinel/dummy nodes to simplify edge cases.

### Stacks & Queues
- Stack: LIFO — monotonic stack pattern (next greater element).
- Queue: FIFO — BFS, sliding window maximum.
- Deque: double-ended, O(1) both ends.
- **Monotonic Stack Template:**
```python
def nextGreaterElement(nums):
    result = [-1] * len(nums)
    stack = []  # stores indices
    for i, val in enumerate(nums):
        while stack and nums[stack[-1]] < val:
            result[stack.pop()] = val
        stack.append(i)
    return result
```

### Hash Tables
- Average O(1) get/set, worst O(n).
- Know: open addressing vs. chaining.
- Collision handling, load factor, rehashing.
- Use cases: frequency maps, two-sum variants, anagram detection.

### Trees
| Type | Key Property | Common Problems |
|---|---|---|
| Binary Tree | Max 2 children | DFS/BFS traversals, LCA |
| BST | left < root < right | Range queries, floor/ceil |
| AVL / Red-Black | Self-balancing | O(log n) guarantee |
| Segment Tree | Range queries | Sum/min/max over range |
| Trie (Prefix Tree) | String prefix lookups | Autocomplete, word search |
| Heap (Priority Queue) | Min/max at root O(1) | Top-K, Dijkstra, merge K lists |

**Tree Traversal Templates:**
```python
# Iterative In-Order (BST sorted output)
def inorder(root):
    stack, result = [], []
    curr = root
    while curr or stack:
        while curr:
            stack.append(curr)
            curr = curr.left
        curr = stack.pop()
        result.append(curr.val)
        curr = curr.right
    return result
```

### Graphs
- **Representations:** Adjacency list (sparse, preferred) vs. adjacency matrix (dense).
- **Traversals:** BFS (shortest path unweighted), DFS (cycle detection, topo sort).
- **Key Algorithms:** Dijkstra, Bellman-Ford, Floyd-Warshall, Prim's, Kruskal's, Tarjan's (SCC).
- **Union-Find (Disjoint Set Union):**
```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # path compression
        return self.parent[x]

    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py: return False
        if self.rank[px] < self.rank[py]: px, py = py, px
        self.parent[py] = px
        if self.rank[px] == self.rank[py]: self.rank[px] += 1
        return True
```

---

## 4. Algorithms & Complexity

### Complexity Cheat Sheet
| Complexity | Name | Example |
|---|---|---|
| O(1) | Constant | Hash lookup |
| O(log n) | Logarithmic | Binary search |
| O(n) | Linear | Linear scan |
| O(n log n) | Linearithmic | Merge sort |
| O(n²) | Quadratic | Bubble sort |
| O(2ⁿ) | Exponential | Subsets |
| O(n!) | Factorial | Permutations |

### Sorting
- **Merge Sort:** O(n log n), stable, O(n) space. Good for linked lists.
- **Quick Sort:** O(n log n) avg, O(n²) worst, O(log n) space. Great cache performance.
- **Heap Sort:** O(n log n), O(1) space, not stable.
- **Counting/Radix Sort:** O(n + k), for bounded integers.
- Know when to use which. Interviewers may ask *why*.

### Binary Search
```python
# Standard template — find leftmost position
def binary_search(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo <= hi:
        mid = lo + (hi - lo) // 2  # avoids overflow
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1
```
Know: search on answer space (not just arrays), rotated sorted arrays, finding first/last occurrence.

### Dynamic Programming (DP)
**Steps to solve any DP problem:**
1. Identify if it has **optimal substructure** + **overlapping subproblems**.
2. Define the **state** (what changes between subproblems).
3. Write the **recurrence relation**.
4. Decide **top-down** (memoization) or **bottom-up** (tabulation).
5. Optimize space if possible (rolling array).

**Classic DP Patterns:**
| Pattern | Example Problems |
|---|---|
| 0/1 Knapsack | Subset sum, partition equal subset |
| Unbounded Knapsack | Coin change, rod cutting |
| LCS / Edit Distance | Longest common subsequence, word distance |
| LIS | Longest increasing subsequence |
| Matrix DP | Unique paths, maximal square |
| Interval DP | Burst balloons, matrix chain multiplication |
| Tree DP | House robber III, diameter of tree |

---

## 5. Coding Interview Patterns

### Pattern 1: Two Pointers
Use when: sorted array, pair/triplet sum, palindrome check, removing duplicates.
```python
# 3Sum
def threeSum(nums):
    nums.sort()
    result = []
    for i in range(len(nums) - 2):
        if i > 0 and nums[i] == nums[i-1]: continue
        l, r = i + 1, len(nums) - 1
        while l < r:
            s = nums[i] + nums[l] + nums[r]
            if s == 0: result.append([nums[i], nums[l], nums[r]]); l += 1; r -= 1
            elif s < 0: l += 1
            else: r -= 1
    return result
```

### Pattern 2: Sliding Window
Use when: subarray/substring with a constraint (max sum, at most K distinct, anagram).
```python
# Longest substring without repeating characters
def lengthOfLongestSubstring(s):
    char_set = set()
    left = max_len = 0
    for right in range(len(s)):
        while s[right] in char_set:
            char_set.remove(s[left]); left += 1
        char_set.add(s[right])
        max_len = max(max_len, right - left + 1)
    return max_len
```

### Pattern 3: BFS / Shortest Path
Use when: min steps, level-order traversal, word ladder, grid problems.

### Pattern 4: DFS / Backtracking
Use when: permutations, combinations, subsets, N-queens, maze solving.
```python
# Subset template
def subsets(nums):
    result = []
    def backtrack(start, path):
        result.append(path[:])
        for i in range(start, len(nums)):
            path.append(nums[i])
            backtrack(i + 1, path)
            path.pop()
    backtrack(0, [])
    return result
```

### Pattern 5: Merge Intervals
Use when: booking systems, meeting rooms, calendar overlap.

### Pattern 6: Top-K Elements (Heap)
Use when: K largest/smallest/most frequent, median of stream.

### Pattern 7: Topological Sort
Use when: course scheduling, task dependencies, build order.

### Pattern 8: Fast & Slow Pointers
Use when: cycle detection, finding middle of linked list, happy number.

---

## 6. System Design Fundamentals

### The RESHADED Framework
A structured approach for any system design interview:

| Letter | Step |
|---|---|
| **R** | Requirements (functional & non-functional) |
| **E** | Estimation (scale, QPS, storage) |
| **S** | Storage (data model, DB choice) |
| **H** | High-level design |
| **A** | APIs |
| **D** | Deep dives (bottlenecks, scaling) |
| **E** | Edge cases |
| **D** | Discussion of trade-offs |

### Step 1: Gather Requirements
**Always clarify before designing.**
```
Functional:
- What are the core features? (MVP vs. full scope)
- Who are the users?
- What does the system do?

Non-Functional:
- Scale (DAU, QPS)?
- Latency requirements (p99)?
- Consistency vs. availability trade-offs?
- Durability / data retention?
- Geo-distribution?
```

### Step 2: Capacity Estimation
**Key numbers every engineer must know:**
```
Latency:
  L1 cache: ~1 ns
  RAM access: ~100 ns
  SSD read: ~100 µs
  Network (same DC): ~500 µs
  Network (cross-region): ~150 ms
  HDD seek: ~10 ms

Throughput:
  SSD: ~500 MB/s read
  Network: 1–10 Gbps (inter-DC)
  RAM: ~50 GB/s

Storage:
  1 KB = 10³ bytes
  1 MB = 10⁶ bytes
  1 GB = 10⁹ bytes
  1 TB = 10¹² bytes

Scale:
  1M req/day  ≈ 12 req/s
  10M req/day ≈ 116 req/s
  1B req/day  ≈ 11,600 req/s
```

**Sample estimation (URL Shortener):**
```
100M new URLs/day → 1,160 writes/s
10:1 read ratio  → 11,600 reads/s
Avg URL size: 100 bytes → 100M * 100B = 10 GB/day new storage
5-year retention: 10 GB * 365 * 5 ≈ 18 TB total
```

### Step 3: Data Modeling
- Draw entities and relationships (ERD mentally).
- Choose SQL vs. NoSQL based on access patterns.
- Think about primary keys, secondary indexes, sharding keys.

### Step 4: High-Level Architecture
Components to consider:
- **Client** (web, mobile, CLI)
- **CDN** (static assets, edge caching)
- **API Gateway / Load Balancer**
- **Application Servers** (stateless preferred)
- **Cache Layer** (Redis, Memcached)
- **Message Queue** (Kafka, RabbitMQ, SQS)
- **Database** (Primary + Replicas)
- **Object Storage** (S3, GCS)
- **Search Engine** (Elasticsearch, Solr)
- **Monitoring / Alerting**

---

## 7. System Design Deep Dives

### Caching
**Where to cache:**
- **Client-side:** Browser cache, local storage.
- **CDN:** Cloudflare, Akamai — static/semi-static content.
- **Application cache:** In-memory (Redis, Memcached).
- **DB query cache:** Read replicas, materialized views.

**Eviction Policies:**
- **LRU** (Least Recently Used) — most common.
- **LFU** (Least Frequently Used) — better for skewed access.
- **TTL-based** — time-to-live expiry.

**Cache Patterns:**
| Pattern | Description | Risk |
|---|---|---|
| Cache-aside (lazy) | App reads cache, misses hit DB | Cache miss storm |
| Write-through | Write cache + DB together | Write latency |
| Write-behind | Write cache, async DB | Data loss risk |
| Read-through | Cache handles DB on miss | Complexity |

**Cache Problems:**
- **Cache stampede:** Many misses simultaneously. Fix: mutex lock, probabilistic early expiry.
- **Cache penetration:** Query for non-existent key. Fix: bloom filter, null value caching.
- **Cache avalanche:** Mass expiry at same time. Fix: jitter on TTL, persistent caching.

### Load Balancing
**Algorithms:**
- **Round Robin** — equal distribution.
- **Weighted Round Robin** — heterogeneous servers.
- **Least Connections** — best for variable request times.
- **IP Hash** — session stickiness.
- **Consistent Hashing** — minimal redistribution on node change.

**L4 vs. L7 Load Balancing:**
- **L4** (Transport): Routes by IP/TCP. Fast, no content inspection. (e.g., AWS NLB)
- **L7** (Application): Routes by HTTP headers, URL, cookies. More intelligent. (e.g., AWS ALB)

### Consistent Hashing
Used in distributed caches (Memcached), CDNs, distributed DBs.
- Virtual nodes (vnodes) improve load distribution.
- Adding/removing a node only affects adjacent keys.
- Essential for Dynamo-style databases.

### Message Queues & Event Streaming
| Feature | Kafka | RabbitMQ | SQS |
|---|---|---|---|
| Model | Log/stream (pull) | Queue (push) | Queue (pull) |
| Retention | Configurable (days) | Until consumed | Until consumed (14 days) |
| Ordering | Per-partition | Per-queue (FIFO) | FIFO queues only |
| Throughput | Very high | High | High (managed) |
| Replayability | Yes | No (by default) | No |
| Use case | Event streaming, analytics | Task queues, RPC | Decoupled AWS services |

**When to use a queue:**
- Decouple producers from consumers.
- Handle traffic spikes (buffering).
- Background job processing.
- Guaranteed delivery semantics.

### API Design
**REST best practices:**
```
GET    /users/{id}        → get user
POST   /users             → create user
PUT    /users/{id}        → full update
PATCH  /users/{id}        → partial update
DELETE /users/{id}        → delete user
```

**GraphQL vs. REST vs. gRPC:**
| | REST | GraphQL | gRPC |
|---|---|---|---|
| Protocol | HTTP/JSON | HTTP/JSON | HTTP/2 + Protobuf |
| Flexibility | Fixed schema | Client-defined | Fixed schema |
| Performance | Good | Good | Excellent |
| Use case | Public APIs | Complex UIs | Internal microservices |
| Streaming | Limited | Subscriptions | Bidirectional |

**Pagination strategies:**
- **Offset pagination:** `?page=2&limit=20` — simple but inconsistent with mutations.
- **Cursor pagination:** `?cursor=xyz&limit=20` — stable, ideal for real-time feeds.
- **Keyset pagination:** Uses DB index — most efficient at scale.

---

## 8. Software Architecture Patterns

### Monolith vs. Microservices
| | Monolith | Microservices |
|---|---|---|
| Deployment | Single unit | Independent services |
| Scalability | Scale whole app | Scale per service |
| Development | Simple initially | Complex coordination |
| Failure isolation | Low | High |
| Data management | Single DB | DB per service |
| Best for | Small teams, early stage | Large teams, scale |

**When NOT to use microservices:**
- Small team (< 5 engineers).
- Unclear domain boundaries.
- Insufficient DevOps maturity.
- Strong consistency requirements across domains.

### Domain-Driven Design (DDD)
**Core concepts:**
- **Bounded Context:** Clear boundary where a domain model applies.
- **Ubiquitous Language:** Shared vocabulary between devs and domain experts.
- **Aggregates:** Cluster of domain objects treated as a unit (has a root entity).
- **Domain Events:** Something that happened in the domain (past tense: `OrderPlaced`).
- **Repositories:** Interface to access aggregates.
- **CQRS:** Separate read and write models.

### Event-Driven Architecture
```
Producer → [Event Broker] → Consumer(s)
                 ↓
         [Event Store / Log]
```
**Patterns:**
- **Event Sourcing:** Store state as a sequence of events. Full audit trail. Complex queries.
- **CQRS:** Command Query Responsibility Segregation — separate write/read models.
- **Saga Pattern:** Manage distributed transactions via choreography or orchestration.

**Saga: Choreography vs. Orchestration**
- **Choreography:** Services react to events independently (decoupled, harder to trace).
- **Orchestration:** Central orchestrator commands each step (easier to trace, coupling risk).

### Clean Architecture / Hexagonal Architecture
```
[UI / API / CLI]   ←→   [Application Layer]   ←→   [Domain Layer]
                                ↕
                     [Infrastructure Layer]
                    (DB, External APIs, Cache)
```
- **Dependency Rule:** Source code dependencies only point inward.
- **Domain layer** has zero external dependencies.
- **Ports:** Interfaces defined by the application.
- **Adapters:** Implementations of ports (DB, REST, gRPC, etc.).

### SOLID Principles
| Principle | Meaning | Violation Smell |
|---|---|---|
| **S** — Single Responsibility | One class, one reason to change | God class |
| **O** — Open/Closed | Open for extension, closed for modification | Switch on type |
| **L** — Liskov Substitution | Subclass must be substitutable for base | Broken overrides |
| **I** — Interface Segregation | No client should depend on unused methods | Fat interfaces |
| **D** — Dependency Inversion | Depend on abstractions, not concretions | `new` in business logic |

### Design Patterns (Key Ones for Interviews)
**Creational:**
- **Singleton:** One instance (use sparingly; makes testing hard).
- **Factory / Abstract Factory:** Decouple object creation.
- **Builder:** Step-by-step object construction.

**Structural:**
- **Adapter:** Convert interface to another.
- **Decorator:** Add behavior dynamically.
- **Proxy:** Control access (lazy loading, caching, auth).
- **Facade:** Simplify complex subsystem.

**Behavioral:**
- **Observer:** Pub/sub event notifications.
- **Strategy:** Swap algorithms at runtime.
- **Command:** Encapsulate request as object (undo/redo).
- **Circuit Breaker:** Prevent cascade failures (resilience pattern).

---

## 9. Databases & Storage

### SQL vs. NoSQL Decision Tree
```
Need ACID transactions?           → SQL (PostgreSQL, MySQL)
Need horizontal write scaling?    → NoSQL
Need flexible schema?             → Document DB (MongoDB)
Need ultra-low latency KV?        → Redis, DynamoDB
Need time-series data?            → InfluxDB, TimescaleDB
Need full-text search?            → Elasticsearch
Need graph relationships?         → Neo4j, Amazon Neptune
Need high write throughput/logs?  → Cassandra, HBase
```

### SQL Deep Dive
**Indexing:**
- **B-Tree index:** Default. Good for equality and range queries.
- **Hash index:** Only equality. Memory-based.
- **GIN/GiST:** Full-text, JSONB, array columns (PostgreSQL).
- **Composite index:** Column order matters. `(a, b)` helps `WHERE a=x` and `WHERE a=x AND b=y`.
- **Covering index:** Index includes all columns needed for query (avoids table fetch).

**Query Optimization:**
- Use `EXPLAIN ANALYZE` to see execution plan.
- Avoid `SELECT *` — select only needed columns.
- Avoid functions on indexed columns in WHERE (`WHERE YEAR(created_at) = 2024` breaks index).
- N+1 query problem: use JOINs or batch loading.

**Transactions & Isolation Levels:**
| Level | Dirty Read | Non-Repeatable Read | Phantom Read |
|---|---|---|---|
| Read Uncommitted | ✓ | ✓ | ✓ |
| Read Committed | ✗ | ✓ | ✓ |
| Repeatable Read | ✗ | ✗ | ✓ |
| Serializable | ✗ | ✗ | ✗ |

### NoSQL Patterns
**DynamoDB / Cassandra key design:**
- **Partition key:** Distributes data evenly. Avoid hot partitions.
- **Sort key:** Enables range queries within a partition.
- Design table around access patterns, not entity relationships.

**MongoDB:**
- Embed vs. reference: embed for data accessed together, reference for large/shared data.
- Aggregation pipeline for complex queries.

### Database Scaling Strategies
```
Vertical scaling        → Bigger machine (limits exist)
Read replicas           → Scale read-heavy workloads
Caching                 → Reduce DB load entirely
Sharding (horizontal)   → Partition data across nodes
  ├── Range sharding    → Easy range queries, hot spots risk
  ├── Hash sharding     → Even distribution, no range queries
  └── Directory sharding→ Lookup service, flexible
Federated DBs           → Split by function (users DB, products DB)
```

---

## 10. Distributed Systems

### CAP Theorem
A distributed system can only guarantee **2 of 3:**
- **C**onsistency — Every read gets the most recent write.
- **A**vailability — Every request gets a (non-error) response.
- **P**artition Tolerance — System works despite network partitions.

> Since network partitions are inevitable, the real choice is **CP vs. AP**.

**PACELC Extension:**
Even without partitions: trade-off between **Latency** and **Consistency**.

### Replication
**Leader-Follower (Primary-Replica):**
- All writes go to leader, replicated async (or sync) to followers.
- Reads can be served by followers (eventual consistency).
- Failover: promote a follower (split-brain risk).

**Leaderless (Dynamo-style):**
- Any node can accept writes.
- Use quorum reads/writes: W + R > N for strong consistency.
- Conflict resolution: last-write-wins (LWW), vector clocks, CRDTs.

### Consensus Algorithms
- **Raft:** Understandable leader election + log replication. Used in etcd, CockroachDB.
- **Paxos:** Theoretical foundation, complex. Used in Chubby (Google).
- **Zookeeper (ZAB):** Atomic broadcast for coordination.

### Distributed Transactions
- **2-Phase Commit (2PC):** Coordinator asks all nodes to prepare, then commit. Simple but blocking.
- **3-Phase Commit (3PC):** Adds pre-commit phase to reduce blocking.
- **Saga Pattern:** Break into local transactions with compensating actions.

### Idempotency
Every distributed operation should be safe to retry.
- Use idempotency keys (UUID per request).
- Design APIs so duplicate requests have the same result.
- Essential for payment systems, order processing.

### Rate Limiting Algorithms
| Algorithm | Description | Pros/Cons |
|---|---|---|
| Token Bucket | Tokens added at rate R, consumed per request | Allows bursts |
| Leaky Bucket | Queue of fixed size, process at constant rate | Smooths traffic |
| Fixed Window | Count requests per time window | Edge spikes at window boundary |
| Sliding Window Log | Track timestamps of requests | Accurate, high memory |
| Sliding Window Counter | Interpolate between windows | Approximation, efficient |

---

## 11. Reliability & Observability

### SLA / SLO / SLI
- **SLI** (Service Level Indicator): The metric (latency p99, error rate).
- **SLO** (Service Level Objective): The target (p99 latency < 200ms, 99.9% uptime).
- **SLA** (Service Level Agreement): The contract with consequences.

### Error Budget
```
SLO: 99.9% uptime
Allowed downtime/month: 0.1% × 30 days × 24h × 60m = 43.2 minutes/month
```

### The Four Golden Signals (Google SRE)
1. **Latency** — Time to serve a request (distinguish success vs. error latency).
2. **Traffic** — Demand on your system (RPS, active users).
3. **Errors** — Rate of failed requests (5xx, timeouts, wrong results).
4. **Saturation** — How "full" your service is (CPU %, queue depth, disk usage).

### Observability Stack
```
Logs       → Structured logging (JSON), log aggregation (ELK, Splunk, Cloud Logging)
Metrics    → Time-series (Prometheus, Datadog, CloudWatch)
Traces     → Distributed tracing (Jaeger, Zipkin, Cloud Trace)
Alerts     → PagerDuty, OpsGenie, incident management
Dashboards → Grafana, Looker
```

### Failure Handling Patterns
- **Retry with exponential backoff + jitter:** Avoid thundering herd.
- **Circuit Breaker:** Open/half-open/closed states. Fail fast to prevent cascade.
- **Bulkhead:** Isolate failures to a partition (separate thread pools per dependency).
- **Timeout:** Always set timeouts. Cascading hangs kill systems.
- **Fallback:** Return cached/default response when dependency fails.
- **Health checks:** Liveness (is the process alive?) vs. readiness (is it ready for traffic?).

### Disaster Recovery
- **RTO** (Recovery Time Objective): Max tolerable downtime.
- **RPO** (Recovery Point Objective): Max tolerable data loss.
- **Strategies:**
  - Backup & Restore (cheapest, slowest)
  - Pilot Light (minimal standby)
  - Warm Standby (reduced-capacity standby)
  - Multi-Site Active-Active (most expensive, fastest)

---

## 12. Security Fundamentals

### OWASP Top 10 (Know These)
1. Broken Access Control
2. Cryptographic Failures
3. Injection (SQL, XSS, etc.)
4. Insecure Design
5. Security Misconfiguration
6. Vulnerable & Outdated Components
7. Identification & Authentication Failures
8. Software & Data Integrity Failures
9. Logging & Monitoring Failures
10. Server-Side Request Forgery (SSRF)

### Authentication & Authorization
- **AuthN** (Authentication): Who are you? (JWT, OAuth2, SAML, WebAuthn)
- **AuthZ** (Authorization): What can you do? (RBAC, ABAC, ACL)

**JWT Best Practices:**
- Short-lived access tokens (15 min).
- Long-lived refresh tokens stored in httpOnly cookie.
- Validate signature AND expiry AND issuer.
- Never store sensitive data in JWT payload (it's base64, not encrypted).

**OAuth2 Flows:**
- **Authorization Code + PKCE:** Web/mobile apps (most secure).
- **Client Credentials:** Machine-to-machine (M2M).
- **Device Flow:** CLIs, smart TVs.

### Encryption
- **In transit:** TLS 1.3 minimum.
- **At rest:** AES-256.
- **Passwords:** bcrypt / scrypt / Argon2 (never MD5/SHA1 alone).
- **Secrets management:** Vault, AWS Secrets Manager. Never in env vars or code.

---

## 13. Leadership & Behavioral Questions

### The STAR Method
- **S** — Situation: Set the scene.
- **T** — Task: What was your responsibility?
- **A** — Action: What did YOU specifically do?
- **R** — Result: What was the measurable outcome?

### Google Leadership Principles You'll Be Evaluated On
- **Googleyness:** Intellectual humility, comfortable with ambiguity, collaborative.
- **Leadership:** Influencing without authority, mentoring, driving consensus.
- **Role-Related Knowledge:** Technical depth appropriate for the level.

### Common Behavioral Questions & Strong Signals

**"Tell me about a time you disagreed with your team/manager."**
> Show: You voiced your opinion with data, listened to others, committed to the decision, and reflected on the outcome.

**"Describe a project you led end-to-end."**
> Show: Scoping, cross-functional alignment, technical decision-making, delivery, post-mortem.

**"Tell me about a time you failed."**
> Show: Ownership, root cause analysis, what you changed, no blame-shifting.

**"How do you handle technical debt?"**
> Show: You quantify the cost, advocate with business impact framing, prioritize iteratively.

**"Tell me about a time you influenced without authority."**
> Show: Built consensus through data, prototypes, or trusted relationships.

### Questions to Ask Your Interviewer
```
- What does success look like for this role in the first 6 months?
- What's the biggest technical challenge the team is facing?
- How do you balance feature development with reliability/tech debt?
- How are technical decisions made? Who has the final say?
- What does the on-call rotation look like?
- How does the team handle post-mortems?
```

---

## 14. Language-Specific Deep Dives

### Python
- GIL: Threading vs. multiprocessing for CPU-bound tasks.
- `asyncio`: Event loop, coroutines, `async/await` for I/O-bound.
- Generators & iterators: lazy evaluation, memory efficiency.
- Decorators, context managers, metaclasses.
- Memory model: reference counting + cyclic GC.
- Type hints + `mypy` for static analysis.

### Java / Kotlin
- JVM internals: JIT compilation, garbage collectors (G1, ZGC, Shenandoah).
- Concurrency: `ExecutorService`, `CompletableFuture`, `volatile`, `synchronized`.
- Java memory model: happens-before relationship.
- Spring Boot: IoC, AOP, bean lifecycle.
- Virtual threads (Project Loom — Java 21+).

### Go
- Goroutines: lightweight threads (M:N scheduling).
- Channels: communication between goroutines.
- `select` statement for multiplexing.
- No inheritance, favor composition + interfaces.
- Garbage collector: tri-color mark-and-sweep.
- Memory safety without a type hierarchy.

### JavaScript / TypeScript
- Event loop: call stack, microtask queue, macrotask queue.
- Promises vs. async/await vs. callbacks.
- Closures, prototype chain, `this` binding.
- V8 optimizations: hidden classes, inline caching.
- TypeScript: structural typing, generics, decorators.

---

## 15. Practice Resources & Study Plan

### Recommended Resources

**Coding:**
- 📘 [LeetCode](https://leetcode.com) — Primary practice platform. Target: 150+ problems.
- 📘 [NeetCode.io](https://neetcode.io) — Curated list + video explanations.
- 📗 *Cracking the Coding Interview* — Gayle Laakmann McDowell.
- 📗 *Elements of Programming Interviews* — Adnan Aziz.

**System Design:**
- 📘 [System Design Primer](https://github.com/donnemartin/system-design-primer) — GitHub, free.
- 📘 [ByteByteGo](https://bytebytego.com) — Alex Xu's newsletter & book.
- 📗 *Designing Data-Intensive Applications* (DDIA) — Martin Kleppmann. **Essential reading.**
- 📗 *Building Microservices* — Sam Newman.
- 📘 [High Scalability Blog](http://highscalability.com) — Real-world architecture case studies.

**Architecture:**
- 📗 *Clean Architecture* — Robert C. Martin.
- 📗 *Domain-Driven Design* — Eric Evans (the "blue book").
- 📗 *Implementing Domain-Driven Design* — Vaughn Vernon (more practical).
- 📗 *Release It!* — Michael Nygard (resilience patterns).

**Google-Specific:**
- 📘 [Google SRE Book](https://sre.google/books/) — Free online. Read chapters 1–5, 13, 17–19.
- 📘 [Google Engineering Practices](https://google.github.io/eng-practices/) — Code review standards.

### 12-Week Study Plan

```
Weeks 1–2: Foundations
  - Arrays, Strings, Hash Maps, Two Pointers, Sliding Window
  - 3 LeetCode Easy per day, 1 Medium per day
  - Read DDIA Chapter 1–3

Weeks 3–4: Trees & Graphs
  - Binary trees, BST, Heaps, Trie
  - DFS, BFS, Union-Find, Topological Sort
  - 2 Mediums + 1 Hard per day
  - Read DDIA Chapter 5–6 (Replication, Partitioning)

Weeks 5–6: Advanced Algorithms
  - Dynamic Programming (all patterns)
  - Backtracking, Greedy, Divide & Conquer
  - 1–2 Hards per day, revisit weak spots
  - Read DDIA Chapter 7–9 (Transactions, Consistency)

Weeks 7–8: System Design Fundamentals
  - RESHADED framework, capacity estimation
  - Design: URL Shortener, Rate Limiter, Key-Value Store
  - Read ByteByteGo Vol. 1 (Ch. 1–8)
  - Watch Gaurav Sen / InfoQ talks

Weeks 9–10: System Design Advanced
  - Design: YouTube, Twitter Feed, Uber, WhatsApp, Google Search
  - Deep dive: Kafka, distributed transactions, consensus
  - Read DDIA Chapter 11–12 (Stream processing, Future)

Weeks 11–12: Mock Interviews & Polish
  - 2 full mock coding interviews per week (Pramp, Interviewing.io)
  - 1 mock system design per week
  - Prepare 10 STAR stories
  - Review all weak areas
  - Read Google SRE book selected chapters
```

### LeetCode Target List by Pattern
```
Two Pointers:    #1, #15, #42, #11, #167
Sliding Window:  #3, #76, #239, #567, #424
Binary Search:   #33, #153, #4, #74, #162
Trees:           #104, #226, #543, #235, #297
Graphs:          #200, #133, #207, #310, #399
DP:              #70, #198, #300, #1143, #312, #10
Heap:            #215, #347, #295, #621, #23
Backtracking:    #46, #78, #39, #51, #131
```

---

## 🎯 Final Checklist Before Your Interview

### 48 Hours Before
- [ ] Review your top 5 STAR stories.
- [ ] Re-solve 3–5 favorite problems without looking.
- [ ] Review your system design framework (RESHADED).
- [ ] Sleep well. Seriously.

### Day Of
- [ ] Have water and something to write on (whiteboard or paper).
- [ ] Test your audio/camera (for remote).
- [ ] Log in 10 minutes early.
- [ ] Take a deep breath. You've prepared for this.

### During Coding
- [ ] Restate the problem in your own words.
- [ ] Ask 2–3 clarifying questions.
- [ ] Give brute force O complexity, then optimize.
- [ ] Write clean code (meaningful names, edge case comments).
- [ ] Trace through your solution with an example.
- [ ] State final complexity.

### During System Design
- [ ] Drive the conversation.
- [ ] Start with requirements (spend 5 min here).
- [ ] Draw a diagram as you talk.
- [ ] Call out trade-offs explicitly.
- [ ] Address bottlenecks before being asked.
- [ ] Keep an eye on the clock (leave 10 min for deep dives).

---

> *"The best engineers are not those who know all the answers — they are those who ask the right questions, reason clearly under pressure, and communicate their thinking with precision and confidence."*

---

**⭐ Star this repo if it helped you. Good luck!**
