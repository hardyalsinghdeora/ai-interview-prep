# Redis & caching

Almost every backend has a cache, and most production incidents involving one come from invalidation, stampedes or treating Redis as something it is not. Interviewers use it to test whether you can reason about consistency, memory and failure rather than saying "add Redis". Senior backend and system design loops ask most, and Forward Deployed Engineers meet it whenever a customer's slow system needs a quick and safe improvement.

> Related: [System design](../topics/03-system-design/README.md) · [Debugging & troubleshooting](../topics/12-debugging-troubleshooting/README.md) · [Large-scale distributed systems](large-scale-distributed-systems.md) · [API design & microservices](api-design-and-microservices.md)

---

## A. Fundamentals — must answer crisply
- [ ] Why is Redis fast, and what does its largely single-threaded command execution mean for the commands you choose? *(verify)*
- [ ] Strings, hashes, lists, sets and sorted sets: give one real use for each and the complexity of its key operations.
- [ ] Cache-aside vs. read-through vs. write-through vs. write-behind: how does each work, and what does each risk?
- [ ] How do TTLs work, and how does Redis actually expire keys (lazy and active expiry)?
- [ ] What happens when Redis reaches `maxmemory`? Compare the main eviction policies, including `noeviction`, `allkeys-lru` and `volatile-ttl`. *(verify)*
- [ ] RDB vs. AOF: what does each persist, what can you lose, and when would you use both?
- [ ] What is the difference between Redis replication, Sentinel and Cluster?
- [ ] What are cache penetration, cache breakdown and cache avalanche, and how do they differ?
- [ ] Why is `KEYS *` dangerous in production, and what do you use instead?

## B. Working depth
- [ ] How does Redis `MULTI`/`EXEC` differ from a database transaction, and what does `WATCH` add?
- [ ] When would you use a Lua script or Redis Function instead of a pipeline or transaction, and what are the risks of a long-running script? *(verify)*
- [ ] Redis Streams: explain consumer groups, `XACK`, the pending entries list and `XAUTOCLAIM`. When would you choose Streams over Kafka or Pub/Sub? *(verify)*
- [ ] Why does Pub/Sub lose messages, and when is that acceptable?
- [ ] How does the approximate LRU work, and when would LFU be a better eviction choice? *(verify)*
- [ ] How do you design a cache key so that it is versioned, tenant-safe and easy to invalidate?
- [ ] How does `SET key value NX PX` implement a lock, why must the value be a unique token, and how do you release it safely?
- [ ] Implement rate limiting with fixed window, sliding window and token bucket. What does each cost in memory and accuracy?
- [ ] How do you find and handle big keys and hot keys, and why does `UNLINK` exist alongside `DEL`?
- [ ] How do you keep a client from turning a Redis outage into an application outage (timeouts, circuit breakers, connection pools)?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Redis Cluster: how do hash slots, `MOVED`/`ASK` redirects and hash tags work, and what breaks for multi-key operations?
- [ ] Replication is asynchronous. What can be lost during failover, and what do `WAIT` and `min-replicas-to-write` really guarantee? *(verify)*
- [ ] Redlock: how does it work, what is the fencing-token criticism, and when would you use a database or consensus system instead?
- [ ] Stampede protection: compare request coalescing, per-key locks, stale-while-revalidate and probabilistic early refresh.
- [ ] Why do cache-aside races produce stale data after an update and a concurrent read? How do versioning, delete-on-write and short TTLs help, and what remains unsolved?
- [ ] Invalidation at scale: TTL-only vs. event-driven invalidation from CDC. What are the failure modes of each?
- [ ] Multi-layer caching: in-process, Redis and CDN. How do you reason about hit rates, staleness and invalidation across layers?
- [ ] Memory fragmentation, copy-on-write during `BGSAVE` and fork latency: how do they surface, and how do you size for them? *(verify)*
- [ ] Redis licence changes and the Valkey fork: what changed, and how would you decide what to run? *(verify)*
- [ ] Managed Redis or Valkey vs. self-managed, and Memcached as an alternative: throughput, failover, cost and features. *(verify)*
- [ ] When should you not cache? How do you decide that a cache is hiding a query or data modelling problem?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] A popular product's cache entry expires and the database falls over within seconds. Explain what happened and design the fix.
- [ ] Design a distributed rate limiter for a public API with per-customer quotas across three regions.
- [ ] Redis memory jumps to 95% overnight and latency rises. Walk through how you investigate and what you change.
- [ ] A scheduled job is sometimes running twice despite a Redis lock. Find the possible causes and redesign it so double execution is harmless.
- [ ] Design a real-time leaderboard for 10 million players with top-100 and "my rank" queries, then say how it survives a node loss.
- [ ] A cache-aside layer is added to a price lookup and customers now see old prices after updates. How do you fix it, and what freshness promise do you make to the business?
- [ ] Cache hit rate falls from 95% to 60% after a deploy. How do you find out why, and what would you check first?

## E. Hands-on checks (things to be able to DO)
- [ ] Use `redis-cli --bigkeys`, `MEMORY USAGE`, `SLOWLOG` and `INFO` to profile a running instance. *(verify)*
- [ ] Write a token-bucket rate limiter as an atomic Lua script and test it under concurrent load.
- [ ] Implement a safe lock with a unique token, an atomic release and a fencing token checked by the database.
- [ ] Build a consumer group on Redis Streams, kill a consumer mid-message and reclaim its pending entries.
- [ ] Reproduce a stampede in a load test, then remove it with request coalescing and show the database query counts.
- [ ] Run a small Cluster or Sentinel setup locally, force a failover and record what the client sees and which writes are lost.

---

**Good answers show:**
- A stated freshness requirement before any caching pattern is chosen.
- Clear separation of cache use from durable-store use, with persistence and failover losses stated plainly.
- Atomicity reasoning: which operations are atomic, on which node, and what Cluster does to multi-key work.
- Honesty about distributed locks: they reduce contention, but correctness needs idempotency or fencing.
- Failure-first thinking: stampedes, eviction, big keys, failover and what the application does when Redis is down.
- Measured decisions based on hit rate, memory and database load, including when not to cache at all.

**Prove it:** Put a Redis cache in front of a slow PostgreSQL-backed API with versioned keys, CDC-driven invalidation, stampede protection and a Lua token-bucket rate limiter. Load test it, then kill the primary during the test and flush a hot key on purpose. Publish the hit rate, database load, stale-read window and writes lost during failover, and state the freshness promise those numbers support.
