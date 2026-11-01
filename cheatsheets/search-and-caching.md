# Search and Caching Cheatsheet

Covers Elasticsearch and OpenSearch terms, Redis data types and operations, cache patterns, eviction policies, stampede protection and CDN caching.

> Related: [System design](../topics/03-system-design/README.md) · [System design skill](../skills/system-design.md) · [Vector databases skill](../skills/vector-databases.md) · [system-design.md](system-design.md) · [databases-and-sql.md](databases-and-sql.md) · [networking-and-web.md](networking-and-web.md) · [Elastic stack cheatsheet](elastic-stack.md)

## Search basics

| Term | What it means |
|---|---|
| **Elasticsearch** | A distributed search and analytics engine built on Apache Lucene; licensing has changed over time *(verify)*. |
| **OpenSearch** | An open-source fork of Elasticsearch 7.10 under the Apache 2.0 licence *(verify)*. |
| **Lucene** | The Java search library underneath both Elasticsearch and OpenSearch. |
| **Index** | A logical collection of documents, somewhat like a table, split across shards. |
| **Document** | A JSON record that is stored and searched, the basic unit of an index. |
| **Inverted index** | Maps each term to the documents containing it, which makes full-text search fast. |
| **Doc values** | On-disk columnar field data used for sorting and aggregations. |
| **Near real time** | Documents become searchable shortly after indexing rather than immediately, because of refresh. |

## Text analysis and mapping

| Term | What it means |
|---|---|
| **Analyser** | Turns text into searchable terms through character filters, a tokeniser and token filters; mismatches cause missed results. |
| **Tokeniser** | Splits text into tokens, for example at word boundaries. |
| **Token filter** | Changes tokens, such as lowercasing, stemming or adding synonyms. |
| **Stemming** | Reduces words to a root form, so "running" can match "run". |
| **Mapping** | Defines field types and how they are indexed, much like a schema. |
| **Dynamic mapping** | Infers field types automatically, which is convenient but risks wrong types and field explosion. |
| **`text` vs `keyword`** | `text` is analysed for full-text search; `keyword` keeps the exact value for filters, sorting and aggregations. |
| **Mapping explosion** | Too many distinct fields consume cluster memory, usually because of uncontrolled dynamic JSON. |

## Cluster and storage

| Term | What it means |
|---|---|
| **Shard** | A piece of an index, implemented as a Lucene index, which enables horizontal scale. |
| **Primary shard** | Handles writes first; the primary count is fixed at creation unless you split, shrink or reindex. |
| **Replica shard** | A copy that provides failover and more read capacity; its count can change at any time. |
| **Segment** | An immutable file set within a shard; searches read across segments. |
| **Refresh** | Makes newly indexed documents searchable by creating a new segment, one second by default on active indices. |
| **Translog** (transaction log) | A durable record of recent operations, used to recover writes not yet committed to Lucene. |
| **Merge** | Combines segments in the background, reclaiming deleted space but consuming I/O. |
| **Oversharding** | Too many small shards waste cluster resources, a common Elasticsearch operating mistake. |
| **Alias** | A stable name pointing to one or more indices, allowing zero-downtime reindexing. |
| **Reindex** | Copies documents to a new index, often required after an incompatible mapping change. |
| **ILM** (Index Lifecycle Management) | Elasticsearch feature moving indices through hot, warm and cold stages, then deleting them. |
| **Rollover** | Starts a new index by age or size, which keeps logs manageable. |

## Queries and relevance

| Term | What it means |
|---|---|
| **BM25** (Best Match 25) | The default relevance algorithm, using term frequency, rare terms and field length, with saturation limiting repeats. |
| **TF-IDF** (Term Frequency-Inverse Document Frequency) | Scores terms that are frequent in a document but rare overall; the older basis for BM25. |
| **Query context** | Calculates a relevance score: how well a document matches. |
| **Filter context** | A yes or no match with no scoring, which is cacheable and faster. |
| **`match` query** | Analyses the input text, then searches for its terms; the standard full-text query. |
| **`term` query** | Looks for an exact term without analysis; suited to `keyword` fields. |
| **`bool` query** | Combines clauses with `must`, `should`, `must_not` and `filter`. |
| **Aggregation** | Calculates metrics or buckets over results, as used in faceted navigation and dashboards. |
| **`search_after`** | Paginates using the last result's sort values, which avoids costly deep offset paging. |
| **kNN search** (k-Nearest Neighbours search) | Finds the closest vectors, enabling semantic search. |
| **Hybrid search** | Combines keyword and vector results, often using reciprocal rank fusion. |

## Redis data types

| Term | What it means |
|---|---|
| **Redis** (Remote Dictionary Server) | An in-memory data store used for caching and more; Redis 8 added an AGPLv3 licence option *(verify)*. |
| **Valkey** | A Linux Foundation open-source fork of Redis, created after the 2024 licence change *(verify)*. |
| **String** | The basic type holding text, numbers or bytes; supports atomic counters. |
| **List** | An ordered sequence, efficient at both ends, often used for simple queues. |
| **Hash** | Field-value pairs under one key, suited to objects. |
| **Set** | Unique unordered members with fast membership checks. |
| **Sorted set** | Unique members ordered by score, widely used for leaderboards and rate limiters. |
| **Stream** | An append-only log with consumer groups, for lightweight event processing. |
| **Bitmap** | Bit-level operations on strings, a compact way to track flags per user. |
| **HyperLogLog** | Estimates unique counts using very little memory, with a small error. |
| **Geospatial index** | Stores coordinates and supports radius queries, built on sorted sets. |

## Redis operations

| Term | What it means |
|---|---|
| **Single-threaded execution** | Commands run one at a time, giving atomicity, but one slow command blocks others. |
| **RDB** (Redis Database file) | Periodic point-in-time snapshots, which are compact but can lose recent writes. |
| **AOF** (Append-Only File) | Logs write operations for better durability, at the cost of larger files and slower recovery. |
| **Redis replication** | Asynchronous copying to replicas, so a failover can lose recent acknowledged writes. |
| **Sentinel** | Monitors Redis primaries and handles automatic failover without sharding. |
| **Redis Cluster** | Shards data across nodes using 16,384 hash slots, with replicas for failover. |
| **Hash tag** | `{user1}` in keys forces them onto one slot, allowing multi-key operations in Redis Cluster. |
| **Pipelining** | Sends many commands without waiting for each reply, which cuts network round trips. |
| **Lua script** | Runs several commands atomically on the server, useful for check-then-set logic. |
| **`SCAN` vs `KEYS`** | `SCAN` iterates in small steps, while `KEYS` blocks the server, so avoid it in production. |
| **Big key** | A very large value that causes latency spikes; `UNLINK` frees memory without blocking on deletion. |
| **`SET key value NX PX 30000`** | Atomically sets a key only if absent with an expiry, the basic form of a lock. |

## Cache patterns

| Term | What it means |
|---|---|
| **Cache-aside** | The app checks the cache, loads from the database on a miss, then fills the cache; most common pattern. |
| **Read-through** | The cache itself loads missing data from the source, which simplifies application code. |
| **Write-through** | Writes go to cache and store together, improving freshness but slowing writes. |
| **Write-behind** | Writes go to cache first and to the store later, which is fast but can lose data. |
| **Write-around** | Writes skip the cache, avoiding pollution from rarely read data, so first reads miss. |
| **Refresh-ahead** | Reloads popular entries before expiry, reducing latency spikes. |
| **TTL** (Time To Live) | How long an entry stays valid, a simple limit on staleness. |
| **Cache invalidation** | Removing or updating stale entries when source data changes, which is notoriously hard to get right. |
| **Cache hit ratio** | Share of requests served from cache, the main measure of cache effectiveness. |
| **Hot key** | A key receiving disproportionate traffic, which can overload a single cache node. |

## Cache failure modes

| Term | What it means |
|---|---|
| **Cache stampede** | Many requests miss one expired key together and overload the database; also called thundering herd. |
| **Request coalescing** | Lets one request rebuild a missing value while others wait, also known as single flight. |
| **TTL jitter** (Time To Live jitter) | Adds randomness to expiry times, so many keys do not expire together. |
| **Probabilistic early expiry** | Occasionally refreshes a key before expiry, which avoids synchronised rebuilds. |
| **Cache avalanche** | Many keys expire together or a cache fails, so load floods the database. |
| **Cache penetration** | Repeated requests for non-existent data bypass the cache; counter with negative caching or a Bloom filter. |
| **Negative caching** | Stores "not found" results briefly, which protects the source from repeated misses. |

## Eviction policies

| Term | What it means |
|---|---|
| **LRU** (Least Recently Used) | Evicts what has gone longest unused, a good general-purpose default. |
| **LFU** (Least Frequently Used) | Evicts what is accessed least often, which protects steadily popular items. |
| **FIFO** (First-In, First-Out) | Evicts the oldest entry regardless of use; simple, but often less effective. |
| **`noeviction`** | Redis's default policy: writes fail once memory is full, which suits data that must not vanish. |
| **`allkeys-lru`** | Redis applies approximate LRU to all keys, a common choice for pure caches. |
| **`volatile-lru`** | Redis evicts only among keys with an expiry set, protecting persistent keys. |
| **`volatile-ttl`** | Redis evicts keys closest to expiry first. |

## CDN caching

| Term | What it means |
|---|---|
| **CDN** (Content Delivery Network) | Edge servers near users cache content, cutting latency and origin load. |
| **Origin** | The source server a CDN fetches from on a miss. |
| **Edge cache** | Stored content at a CDN location, used to answer nearby users directly. |
| **Origin shield** | An extra cache layer in front of the origin, which reduces load from many edge misses. |
| **`Cache-Control`** | The HTTP header setting caching rules, such as `max-age`, `no-store` or `private`. |
| **`s-maxage`** | Sets the lifetime for shared caches such as CDNs, overriding `max-age` for them. |
| **`stale-while-revalidate`** | Serves stale content while fetching a fresh copy in the background, improving perceived speed. |
| **ETag** (Entity Tag) | A version identifier that allows conditional requests, with `304 Not Modified` saving bandwidth. |
| **`Vary`** | Lists request headers that change the response; overly broad values reduce hit ratio. |
| **Cache key** | What identifies a cached object; including needless query parameters fragments the cache. |
| **Purge** | Explicitly removes content from the CDN, which is needed when TTLs are long. |
| **Cache busting** | Changes file names or versions for each build, so assets can safely be cached for a long time. |
