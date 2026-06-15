# System Design Interview Structure

A repeatable framework for working through a system design interview. Treat the phase order as a default, not a script — surface trade-offs as you go, and let the interviewer redirect you to whichever phase they care about most.

## Suggested Time Budget (45–60 min interview)

| Phase | Time |
|---|---|
| Functional & non-functional requirements | 5–8 min |
| Back-of-envelope estimation | 3–5 min |
| Core entities + APIs | 5–8 min |
| High-level design (boxes & arrows) | 8–12 min |
| Deep dives (driven by interviewer) | 15–25 min |
| Wrap-up: trade-offs & next steps | 2–3 min |

If the interviewer starts redirecting in any phase, follow them — they're signaling what they want to evaluate.

---

## 1. Functional Requirements

Pin down **what the system must do** for its users. Restate the prompt in your own words, then enumerate user-visible capabilities.

**Do:**
- List 3–5 core user actions (e.g., "user posts a tweet", "follower sees it in feed")
- Confirm with interviewer: "Are these the right things to focus on?"
- Force a priority order — primary vs. secondary

**Ask:**
- Who are the users? (consumer, internal, B2B)
- What's the primary flow / golden path?
- What does success look like for a user?

## 2. Out of Scope (Functional)

Equally important — explicitly list what you **won't** design. This buys you focus and signals scoping judgment.

Common things to defer: auth, admin tools, analytics, internationalization, monetization, mobile-specific concerns, legacy migration.

## 3. Non-Functional Requirements

Pin down **how well** the system must do it. The classics:

| Dimension | Question to answer |
|---|---|
| **Scalability** | How many users / req/s at peak? Read-heavy or write-heavy? |
| **Reliability** | What happens if a component fails? Acceptable error rate? |
| **Availability** | Target uptime — 99.9%? 99.99%? What does "down" mean? |
| **Fault tolerance** | Single region vs. multi-region? Graceful degradation? |
| **Latency** | p50, p99 targets for the critical path |
| **Consistency** | Strong, eventual, or read-your-writes? |
| **Durability** | Can data loss ever be acceptable (e.g., logs vs. payments)? |

### CAP positioning

- Distributed systems force a choice during partitions: **CP** (consistency over availability) or **AP** (availability over consistency).
- Default reasoning: financial / inventory / coordination → CP. Feeds / social / search → AP.
- State your pick **and why** — not as dogma, but as a fit for the workload.

## 4. Out-of-Scope Non-Functional

Call out the dimensions you're **not** optimizing for. E.g., "We're prioritizing availability over strong consistency for the feed; eventual consistency is acceptable here."

This shows you understand trade-offs aren't free.

## 5. Guardrails — Back-of-the-Envelope Estimation

Quick math to size the system. See `estimate-facts.md` for reference numbers (DAU, magnitudes, encoding sizes).

**Compute:**
- **QPS (reads)** = DAU × actions/user/day ÷ 86,400 (apply ~10× peak multiplier)
- **WPS (writes)** = same, scaled by write fraction (often 1:100 of reads)
- **Storage** = items × bytes/item × retention period
- **Bandwidth** = QPS × avg payload size

**Sanity checks:**
- Does any single number suggest you need sharding? (Storage > 1 TB, QPS > 10K)
- Is peak / average ratio > 5×? Need load smoothing (queues, autoscaling).

Write the numbers on the board — interviewers will reference them later.

## 6. Core Entities

Identify the **3–5 nouns** the system revolves around. Don't model every field — just names and key relationships.

Example (Twitter): `User`, `Tweet`, `Follow`, `Feed`, `Like`.

For each entity: what owns it, what queries it, what mutates it.

## 7. APIs

Sketch the **handful of endpoints** that cover the functional requirements. REST, gRPC, or GraphQL — pick the fit, justify briefly.

For each:
- Method + path (`POST /tweets`, `GET /feed?user_id=...`)
- Request / response shape (high level — no need for full schemas)
- Idempotency requirements (esp. for writes)
- Pagination strategy (cursor > offset for scale)
- Auth — usually defer to a single sentence ("assume JWT in header")

## 7.5. Data Model

Sketch the schemas / shapes for each core entity. You don't need every field, but **partition strategy, indexes, and key access patterns** should be explicit before you draw the architecture.

**For each core entity capture:**
- Primary key / partition key — what's the access pattern?
- Secondary indexes — derived from the API endpoints you wrote in §7
- Relationships — foreign keys, embedded vs. referenced
- Hot fields — what changes frequently, what's read-heavy
- Approximate row / document size

### SQL vs. NoSQL — which fits

| Store | Pick when |
|---|---|
| **Relational** (Postgres, MySQL) | ACID needs, complex joins, well-known schema, moderate scale |
| **Document** (MongoDB, DynamoDB) | Flexible schema, single-entity reads, scale-out by key |
| **Wide-column** (Cassandra, ScyllaDB) | Massive write throughput, time-series, known access patterns |
| **Key-value** (Redis, DynamoDB) | Simple lookups, low latency, cache-like access |
| **Graph** (Neo4j) | Relationship-heavy queries — recommendations, social |
| **Search** (Elasticsearch, OpenSearch) | Full-text, faceted, ranked queries |
| **Time-series** (TimescaleDB, InfluxDB) | Append-only, time-windowed aggregations |

State your choice **and the access pattern that drove it** — don't justify by familiarity.

### Schema & indexing decisions

- **Partition key** — determines which shard/node stores the row. Wrong choice → hot shard. Pick something with high cardinality and balanced access (avoid `country`, prefer `user_id`).
- **Sort / clustering key** — secondary ordering within a partition; drives efficient range queries.
- **Secondary indexes** — every index is a write tax. Add deliberately, not by default.
- **Denormalization** — duplicate data to avoid joins on the read path. Trade-off: write amplification + harder consistency.
- **Composite keys** — for many-to-many or filtered access (`(user_id, created_at)`).
- **Wide vs. tall rows** — collapse related data into one row vs. split into many; depends on read pattern.

### Common patterns worth naming

- **Outbox pattern** — write to DB and event log atomically for reliable event publishing
- **CQRS** — split read and write models when read patterns diverge from writes
- **Materialized views** — precompute expensive aggregations
- **Event sourcing** — store events, derive state; useful for audit-heavy domains
- **Sharding key vs. routing key** — what determines storage vs. what determines lookup

## 8. Data Flow

Walk through **one or two critical paths** end-to-end before drawing boxes. E.g., "When a user posts a tweet: client → API gateway → write service → DB → fanout service → followers' feed cache."

This forces the design to be specific before you commit to topology.

## 9. High-Level Design

Now draw the boxes and arrows.

**Standard components to consider:**
- Client → CDN → Load balancer → API gateway
- Stateless application services (horizontal scaling)
- Databases (SQL / NoSQL — justify the pick)
- Caches (Redis / Memcached — where, why, eviction policy)
- Message queues / event streams (Kafka, SQS — for async work, fanout, decoupling)
- Object storage (S3 — for blobs, media, backups)
- Search index (Elasticsearch / OpenSearch — when queries don't fit DB indexes)
- Background workers / cron jobs

**Discipline:**
- Every box should have a reason tied to a requirement
- Label arrows with the protocol or message type
- Don't over-draw — leave room for deep-dive elaboration

## 10. Deep Dives

The interviewer will pick 1–3 areas to probe. Be ready for any of these:

### Scalability
- Horizontal vs. vertical scaling — which limit hits first?
- Sharding strategy — by user ID, geography, time? What's the rebalance story?
- Read replicas, write coordinators, consistent hashing
- Hot keys / celebrity problem — how do you avoid one shard taking all traffic?

### Fault Tolerance & Durability
- Replication factor; sync vs. async replication
- Failover behavior — automatic or manual? How long is the gap?
- Backups — frequency, retention, restore time (RTO/RPO)
- Cross-region / multi-AZ strategy
- What does "data loss" look like, and is any acceptable?

### Errors & Retries
- Retry policy: exponential backoff + jitter
- Idempotency keys for safe retries on writes
- Dead-letter queues for messages that fail repeatedly
- Circuit breakers to prevent cascading failures
- Timeouts at every hop — don't let clients hang

### Performance Optimizations
- Caching layers: client, CDN, app, DB query cache — which actually helps?
- Cache invalidation: TTL, write-through, write-behind, explicit purge
- Database indexes — costs and benefits
- Connection pooling, batching, pipelining
- Asynchronous processing — push slow work off the request path
- Compression, protocol choice (gRPC vs. REST vs. WebSocket)

### Retention Policies
- TTL on caches, queues, and DB tables
- Cold storage tiering (hot DB → warm DB → S3 → Glacier)
- Compliance constraints (GDPR right-to-delete, financial audit retention)
- Aggregations vs. raw data — when can you down-sample?

### Consistency
- Strong consistency (transactions, distributed locks)
- Eventual consistency (vector clocks, CRDTs, read repair)
- Read-your-writes — usually achieved with sticky sessions or session tokens
- Quorum reads/writes (W + R > N)
- Saga pattern for distributed transactions

### Race Conditions
- Concurrent writers to the same resource — last-write-wins vs. CAS vs. lock
- Optimistic concurrency control (version numbers) vs. pessimistic (locks)
- Idempotency keys to dedupe parallel client retries
- Be explicit about the invariant being protected

### Stuck Processes / Stale Data
- Workers holding leases that expire — heartbeats and lease renewal
- Tombstones for deleted items in caches and indexes
- Visibility timeouts on queue messages (SQS, Kafka rebalance)
- Watchdog jobs to detect and re-enqueue abandoned work
- Idempotency so re-runs are safe

### DB Capacity
- Estimate: rows × row size × replication factor × retention
- When does one node stop fitting? (commonly ~1–10 TB depending on tech)
- Sharding scheme + resharding plan
- Read/write split across replicas
- Archival path for old data

### DB Atomicity
- ACID transactions: when needed, when overkill
- Distributed transactions: 2PC, sagas, outbox pattern
- Eventual consistency with compensating actions
- Idempotent operations as an alternative to transactions
- Single-shard transactions are cheap; cross-shard are expensive

### High Throughput, Surges & Bursts
- Buffer with queues — smooth bursts into steady processing
- Autoscaling based on queue depth, not CPU alone
- Rate limiting / admission control at the edge
- Backpressure signals propagated upstream
- Pre-warming caches before known spikes (e.g., product launches, market open)
- Shed load gracefully — degrade non-critical features first

---

## Wrap-Up

Close with:
1. **Summary** — one sentence on what you built
2. **Trade-offs you made** — and what would change them
3. **What you'd build next** — the obvious extensions if you had more time

Don't end silently. The wrap-up is where you demonstrate awareness of what your design isn't.

---

## Common Anti-Patterns

- Jumping to architecture before pinning requirements
- Listing technologies (Kafka! Redis! Cassandra!) without justifying fit
- Solving for scale you don't have — over-engineering early
- Ignoring the interviewer's redirect signals and finishing your script
- Drawing complicated diagrams while talking past your interviewer
- Skipping the math — claiming "it'll scale" without numbers
- Not naming what's out of scope — implicitly promising everything