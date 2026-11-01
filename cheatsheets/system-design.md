# System Design Cheatsheet

Core numbers, patterns and building blocks for designing scalable systems, ending with a checklist for system design interviews.

> Related: [distributed-systems.md](distributed-systems.md) · [databases-and-sql.md](databases-and-sql.md) · [networking-and-web.md](networking-and-web.md) · [search-and-caching.md](search-and-caching.md) · [messaging-and-streaming.md](messaging-and-streaming.md) · [observability-and-reliability.md](observability-and-reliability.md) · [System design topic](../topics/03-system-design/README.md) · [System design skill](../skills/system-design.md)

## Latency numbers

| Term | What it means |
|---|---|
| **Latency** | Time taken to serve one request. Users feel it directly, and it often sets the design's hardest constraints. |
| **Throughput** | Work completed per unit of time. Can improve through batching or parallelism even when latency stays the same. |
| **Bandwidth** | Maximum data rate of a link. Distinct from latency: a fast pipe can still have a long round trip. |
| **P50, P95 and P99** (50th, 95th and 99th Percentile) | Value below which that share of requests fall. Reveal slow outliers that averages hide. |
| **Tail latency** | The slowest requests, typically P99 and above. Fan-out magnifies it, because one slow dependency delays the whole response. |

Rough orders of magnitude, for estimation only; real figures vary with hardware *(verify)*.

| Operation | Approx. latency |
|---|---|
| L1 cache reference | 1 ns |
| Branch mispredict | 3–5 ns |
| L2 cache reference | 4 ns |
| Uncontended mutex lock and unlock | 20 ns |
| Main memory reference | 100 ns |
| Compress 1 KB with a fast codec | 2–10 µs |
| Send 1 KB over a 1 Gbps network | 10 µs |
| SSD random read | 20–100 µs |
| Read 1 MB sequentially from memory | 50 µs |
| Round trip within a data centre | 0.1–0.5 ms |
| Read 1 MB sequentially from SSD | 0.3–2 ms |
| Round trip across zones in a region | 1–2 ms |
| Hard disk seek | 5–10 ms |
| Read 1 MB sequentially from hard disk | 10 ms |
| Round trip across continents | 70–150 ms |

Rule of thumb: memory is about 1,000× faster than a network round trip within a data centre, which is hundreds of times faster than crossing continents.

## Capacity estimation

| Term | What it means |
|---|---|
| **Back-of-the-envelope estimate** | Quick rough calculation of load and storage. Shows which parts of a design are actually hard; orders of magnitude are enough. |
| **DAU** (Daily Active Users) | Distinct users active per day. The usual starting point for estimating traffic. |
| **MAU** (Monthly Active Users) | Distinct users active per month. The DAU to MAU ratio indicates how engaged users are. |
| **QPS** (Queries Per Second) | Request rate a system must serve. Drives server, database and cache sizing. |
| **Peak-to-average ratio** | How much busier the peak is than average, often 2–3× or more. Size capacity for the peak, not the mean. |
| **Read-to-write ratio** | Proportion of reads to writes. Read-heavy systems suit caches and replicas; write-heavy systems suit sharding and log-structured stores. |
| **Headroom** | Spare capacity kept above expected peak. Absorbs spikes, failed nodes and growth, commonly aiming for 60–70% utilisation. |
| **Little's Law** | Items in a system = arrival rate × average time inside (`L = λW`). Sizes concurrency, connection pools and queues. |

```text
Seconds per day        86,400 (round to 10^5)
1 million per day      about 12 per second
1 billion per day      about 12,000 per second
Average QPS            DAU × requests per user per day / 86,400
Peak QPS               average QPS × 2 to 3
Storage per year       new items per day × item size × 365 × replicas
Bandwidth              QPS × average response size
Powers of two          2^10 about 10^3 (KB), 2^20 about 10^6 (MB), 2^30 about 10^9 (GB), 2^40 about 10^12 (TB)
```

## Availability and nines

| Term | What it means |
|---|---|
| **Availability** | Share of time or requests a system serves correctly. Each extra nine usually costs far more to achieve. |
| **SLI** (Service Level Indicator) | The measured value, such as the success rate or P99 latency. What monitoring reports. |
| **SLO** (Service Level Objective) | Internal target for an SLI, such as 99.9% success over 30 days. Guides engineering priorities. |
| **SLA** (Service Level Agreement) | Contractual promise to customers, with penalties for missing it. Normally set looser than the SLO. |
| **Error budget** | Permitted unreliability, equal to 100% minus the SLO. Lets teams balance feature velocity against reliability work. |
| **MTBF** (Mean Time Between Failures) | Average time between failures. Higher is better, though failures remain inevitable. |
| **MTTR** (Mean Time To Recovery) | Average time to restore service. Often the most practical lever for improving availability. |
| **RPO** (Recovery Point Objective) | Maximum acceptable data loss, measured in time. Determines backup frequency and replication mode. |
| **RTO** (Recovery Time Objective) | Maximum acceptable time to restore service. Determines how much standby capacity to pay for. |
| **SPOF** (Single Point of Failure) | A component whose failure stops the whole system. Remove with redundancy where the risk justifies the cost. |
| **Active-active** | All sites serve traffic at once. Fast failover and better capacity use, but data conflicts and complexity increase. |
| **Active-passive** | A standby takes over when the primary fails. Simpler, but failover takes time and spare capacity sits idle. |

| Availability | Downtime per year | Downtime per month | Downtime per day |
|---|---|---|---|
| 99% (two nines) | 3.65 days | 7.31 hours | 14.4 min |
| 99.9% (three nines) | 8.77 hours | 43.8 min | 1.44 min |
| 99.95% | 4.38 hours | 21.9 min | 43.2 s |
| 99.99% (four nines) | 52.6 min | 4.38 min | 8.64 s |
| 99.999% (five nines) | 5.26 min | 26.3 s | 0.86 s |

```text
Series (all must work):   A × B              99.9% × 99.9% = 99.8%
Parallel (any can work):  1 - (1 - A)(1 - B)  two 99.9% replicas = 99.9999%, if failures are independent
```

## Scaling patterns

| Term | What it means |
|---|---|
| **Vertical scaling** | Moving to a bigger machine. Simple and needs no redesign, but has a ceiling and remains one failure point. |
| **Horizontal scaling** | Adding more machines. Near-limitless growth and redundancy, but requires load distribution and handling of shared state. |
| **Stateless service** | Instances keep no per-client state between requests. Any instance can serve any request, which makes scaling and replacement easy. |
| **Read replica** | Database copy serving reads. Scales read-heavy workloads cheaply, but clients must tolerate replication lag. |
| **Sharding** | Splitting data across databases by key. Scales writes and storage, at the cost of cross-shard queries and operational complexity. |
| **CQRS** (Command Query Responsibility Segregation) | Separate models for writing and reading. Each side can be optimised independently, but read views are often eventually consistent. |
| **Event sourcing** | Stores state as an append-only sequence of events. Provides audit history and replay, with added schema and query complexity. |
| **Asynchronous processing** | Queues slow work for background workers. Keeps requests fast and smooths spikes, but adds eventual consistency and retries. |
| **Fan-out on write** | Pre-computes each follower's feed when content is posted. Fast reads, but expensive for accounts with huge audiences. |
| **Fan-out on read** | Assembles results when requested. Cheap writes, but slower reads; feeds often combine both approaches. |
| **Autoscaling** | Adds and removes capacity based on metrics. Matches cost to demand, but lags sudden spikes. |
| **Multi-region deployment** | Runs in several geographic regions. Improves latency and disaster tolerance, while data replication and cost become harder. |
| **Cell-based architecture** | Splits a service into isolated full copies serving subsets of users. Limits the blast radius of any failure. |
| **Microservices** | Independently deployable services around business capabilities. Enable team autonomy, but add network failures and operational overhead. |
| **Modular monolith** | One deployable with strong internal boundaries. Keeps simple operations, and is often a better starting point than microservices. |

## Caching patterns

| Term | What it means |
|---|---|
| **Cache-aside** | Application checks the cache, loads from the database on a miss, then fills the cache. Common, simple and resilient to cache failure. |
| **Read-through** | Cache itself loads missing data from the database. Simplifies application code, but needs cache-level loader support. |
| **Write-through** | Writes pass through the cache to the database synchronously. Keeps the cache current, at the cost of write latency. |
| **Write-behind** | Writes go to the cache, then reach the database later. Very fast writes, but a crash can lose data. |
| **Write-around** | Writes go straight to the database, skipping the cache. Avoids filling the cache with unread data, but first reads miss. |
| **Refresh-ahead** | Refreshes popular entries before they expire. Reduces latency spikes, but wastes work if predictions are wrong. |
| **TTL** (Time To Live) | How long an entry stays valid. A simple safety net that bounds staleness when invalidation fails. |
| **Cache invalidation** | Removing or updating entries when data changes. Famously hard, because races can leave stale data behind. |
| **LRU** (Least Recently Used) | Evicts the entry unused for longest. A good general default for typical access patterns. |
| **LFU** (Least Frequently Used) | Evicts the entry accessed least often. Keeps long-term popular items, but adapts slowly to changing patterns. |
| **Cache stampede** | Many requests miss the same expired key at once and hit the database. Prevent with locks, coalescing or jittered expiry. |
| **Cache penetration** | Repeated requests for keys that do not exist always reach the database. Mitigate with negative caching or a Bloom filter. |
| **Cache avalanche** | Many keys expire together, or the cache fails, overloading the database. Mitigate with TTL jitter, redundancy and fallbacks. |
| **Request coalescing** | Concurrent requests for one key share a single backend call. Also called single-flight; greatly reduces load on misses. |
| **Hot key** | One key receives disproportionate traffic. Can overload a single cache node; mitigate with local caching or replicated keys. |
| **Negative caching** | Stores "not found" results, briefly. Protects the database from repeated lookups of missing items. |

| Write pattern | Consistency | Write latency | Data-loss risk |
|---|---|---|---|
| Write-through | Strong between cache and database | Higher | Low |
| Write-behind | Eventual | Lowest | Higher |
| Write-around | Stale until refreshed or expired | Low | Low |
| Cache-aside (delete on write) | Usually fresh, small race window | Low | Low |

## Rate limiting algorithms

| Term | What it means |
|---|---|
| **Rate limiting** | Caps requests per client or key over time. Protects capacity and fairness, usually returning `429` with `Retry-After`. |
| **Token bucket** | Tokens refill at a steady rate and each request spends one. Allows bursts up to bucket size; a common default. |
| **Leaky bucket** | Requests queue and leave at a fixed rate. Produces smooth output, but bursts wait or get dropped. |
| **Fixed window counter** | Counts requests per time block. Very simple, but allows up to double the limit around window boundaries. |
| **Sliding window log** | Stores a timestamp per request and counts those in the window. Exact, but memory-heavy at high volume. |
| **Sliding window counter** | Weights the previous window's count to approximate a rolling window. Good accuracy with minimal memory. |
| **Concurrency limit** | Caps in-flight requests rather than request rate. Protects better against slow requests that consume capacity. |
| **Distributed rate limiting** | Shares counters across instances, often in Redis with atomic scripts. Adds latency, so decide whether to fail open or closed. |

| Algorithm | Allows bursts | Memory | Accuracy |
|---|---|---|---|
| Token bucket | Yes, to bucket size | Low | Good |
| Leaky bucket | No, smooths them | Low to moderate | Good |
| Fixed window | Yes, at boundaries | Very low | Coarse |
| Sliding window log | No | High | Exact |
| Sliding window counter | Limited | Low | Approximate |

## Common building blocks

| Term | What it means |
|---|---|
| **Load balancer** | Distributes traffic across healthy instances. Enables horizontal scaling and removes failed backends from rotation. |
| **API gateway** (Application Programming Interface) | Single entry point handling authentication, rate limits and routing. Keeps cross-cutting concerns out of services. |
| **CDN** (Content Delivery Network) | Serves content from locations near users. Cuts latency and origin load, especially for static media. |
| **Message queue** | Buffers work for consumers, with each message usually handled once. Decouples services and absorbs traffic spikes. |
| **Pub/sub** (Publish-Subscribe) | Publishers send to a topic, and each subscriber gets a copy. Enables fan-out without senders knowing receivers. |
| **Event log** | Durable, ordered, replayable stream, as in Kafka. Lets many consumers read independently and reprocess history. |
| **Object storage** | Durable, cheap storage for files accessed over HTTP. Suits images, video, backups and data lakes, not low-latency updates. |
| **Presigned URL** (Uniform Resource Locator) | Time-limited signed link for accessing an object directly. Clients transfer large files without passing through application servers. |
| **Search index** | Inverted index for full-text and filtered search, as in Elasticsearch. Usually a derived copy fed from the source of truth. |
| **Stream processor** | Computes results continuously over events, as in Flink. Enables near-real-time aggregation, alerting and enrichment. |
| **Service discovery** | Lets services find healthy instances dynamically. Essential when instances change frequently, as in containers. |
| **Workflow engine** | Durably runs long multi-step processes with retries and timers, as in Temporal. Avoids hand-written state machines. |
| **UUID** (Universally Unique Identifier) | 128-bit identifier needing no coordination. Random v4 harms B-tree locality, while time-ordered v7 indexes better. |
| **Snowflake ID** (Identifier) | 64-bit, roughly time-ordered ID combining timestamp, machine number and sequence. Scales generation, but relies on sane clocks. |
| **Bloom filter** | Compact set test with no false negatives but some false positives. Skips expensive lookups for items definitely absent. |
| **HyperLogLog** | Estimates distinct counts using very little memory. Suits unique visitor counts where small error is acceptable. |
| **Count-min sketch** | Estimates item frequency in fixed memory, possibly overcounting. Suits heavy-hitter detection and trending lists. |
| **Geohash** | Encodes location into a string where shared prefixes usually mean closeness. Boundary cases require checking neighbouring cells. |
| **Quadtree** | Recursively divides 2D space into four regions. Adapts to uneven density for nearby-place searches. |

## Design interview checklist

| Term | What it means |
|---|---|
| **Clarify scope** | Agree users, core use cases and what is out of scope. Prevents solving the wrong problem in limited time. |
| **Functional requirements** | The features the system must provide. Keep to the few that drive the architecture. |
| **Non-functional requirements** | Targets for scale, latency, availability, consistency and durability. These, rather than features, shape most of the design. |
| **Estimate scale** | Calculate QPS, storage and bandwidth, including peaks. Round aggressively and use results to justify decisions. |
| **API sketch** (Application Programming Interface) | Outline key endpoints, parameters and pagination. Fixes the contract before internal details are designed. |
| **Data model** | Define main entities, relationships, access patterns and shard key. Choose storage from those access patterns. |
| **High-level architecture** | Draw the main components and request flow end to end. Confirm a simple working version before optimising. |
| **Deep dive** | Explore the hardest one or two components in detail. This is where real engineering depth becomes visible. |
| **Bottlenecks** | Find the first thing to break as load grows, then address it. Add complexity only where numbers demand it. |
| **Failure modes** | Walk through node, zone, dependency and data-corruption failures. Explain timeouts, retries, failover and any data loss. |
| **Trade-offs** | State the options, the choice and what it sacrifices. Connect each decision back to stated requirements. |
| **Operations** | Cover metrics, alerts, rollout, migration and cost. Shows the design can actually be run in production. |
