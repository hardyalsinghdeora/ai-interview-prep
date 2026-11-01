# Databases and SQL Cheatsheet

Core terms for how databases store, protect, query and scale data, covering transactions, indexes, replication, sharding and NoSQL.

> Related: [distributed-systems.md](distributed-systems.md) · [system-design.md](system-design.md) · [data-engineering.md](data-engineering.md) · [search-and-caching.md](search-and-caching.md) · [Data and SQL topic](../topics/07-data-and-sql/README.md) · [PostgreSQL and data modelling skill](../skills/postgresql-and-data-modelling.md)

## Transactions and ACID

| Term | What it means |
|---|---|
| **ACID** (Atomicity, Consistency, Isolation, Durability) | The guarantees of a reliable transaction. Lets applications trust the database rather than handle partial failure and concurrency themselves. |
| **Atomicity** | A transaction fully happens or has no effect. Failed work rolls back, so no partial state is left behind. |
| **Consistency** (ACID) | Each transaction moves data between states that satisfy constraints. Differs from replica consistency in distributed systems. |
| **Isolation** | Concurrent transactions are protected from each other's in-progress work. The chosen level trades correctness against throughput. |
| **Durability** | Once committed, data survives crashes, usually through a flushed log. Depends on real disk flushes and on replication settings. |
| **Optimistic locking** | Commit succeeds only if a version number is unchanged, otherwise retry. Good for low contention, as no locks are held. |
| **Pessimistic locking** | Takes locks before changing data, for example `SELECT ... FOR UPDATE`. Safer under high contention but reduces concurrency. |
| **Database deadlock** | Transactions wait on each other's locks forever. Databases detect this and abort one, so applications must be ready to retry. |

## Isolation levels and anomalies

| Term | What it means |
|---|---|
| **Dirty read** | Reading another transaction's uncommitted data. That data may roll back, so decisions rest on values that never existed. |
| **Non-repeatable read** | Reading the same row twice gives different values because another transaction committed between reads. Breaks calculations spanning several reads. |
| **Phantom read** | Rerunning a range query returns different rows because of a concurrent commit. Row locks alone cannot prevent it. |
| **Lost update** | Two transactions read then write the same value, and one overwrites the other. Atomic updates or version checks prevent it. |
| **Write skew** | Two transactions read overlapping data and update different rows, jointly breaking a rule. Snapshot isolation allows it; full serialisable prevents it. |
| **`READ UNCOMMITTED`** | Weakest level, permitting dirty reads. Rarely useful; PostgreSQL treats it as `READ COMMITTED`. |
| **`READ COMMITTED`** | Each statement sees only committed data as of its start. A common default, including in PostgreSQL *(verify)*. |
| **`REPEATABLE READ`** | Rows already read will not change within the transaction. MySQL InnoDB's default; behaviour differs by engine *(verify)*. |
| **Snapshot isolation** | Each transaction reads a consistent point-in-time view. Readers avoid blocking writers, though write skew remains possible. |
| **`SERIALIZABLE`** | Strongest standard level; outcomes match some one-at-a-time order. Safest for invariants, but expect aborts, retries or more blocking. |
| **SSI** (Serialisable Snapshot Isolation) | Snapshot isolation plus tracking of dangerous read and write dependencies, aborting conflicts. Used by PostgreSQL for `SERIALIZABLE`. |
| **2PL** (Two-Phase Locking) | Acquire locks while growing, release them while shrinking. Classic path to serialisability, at the cost of blocking and deadlocks. |

| Level | Dirty read | Non-repeatable read | Phantom | Write skew |
|---|---|---|---|---|
| Read Uncommitted | Possible | Possible | Possible | Possible |
| Read Committed | No | Possible | Possible | Possible |
| Repeatable Read (standard minimum) | No | No | Possible | Possible |
| Snapshot Isolation | No | No | No | Possible |
| Serializable | No | No | No | No |

## Index types

| Term | What it means |
|---|---|
| **Index** | Extra structure that finds rows without scanning the whole table. Speeds reads, while adding write cost and storage. |
| **B-tree index** | Balanced, sorted tree and the usual default. Supports equality, range queries and ordering in logarithmic time. |
| **Hash index** | Maps a hash of the key to rows. Good for equality lookups only, with no range or ordering support. |
| **GIN** (Generalized Inverted Index) | Maps values within a column, such as words or JSON keys, to rows. Suited to full-text, arrays and `jsonb` in PostgreSQL. |
| **BRIN** (Block Range Index) | Stores summaries per block range. Tiny and effective for huge tables whose values follow physical order, such as timestamps. |
| **LSM tree** (Log-Structured Merge tree) | Buffers writes in memory, then flushes and merges sorted files. Favours heavy writes, with read and compaction overhead. |
| **Composite index** | Index on several columns, in a set order. The leftmost-prefix rule means column order decides which queries it can help. |
| **Covering index** | Contains every column a query needs. Allows an index-only scan, which avoids visiting the table. |
| **Partial index** | Indexes only rows matching a condition, for example active orders. Smaller and cheaper when queries target a subset. |
| **Clustered index** | Table rows are physically stored in index order. Fast range scans on that key, and only one per table. |
| **Selectivity** | How well a condition narrows rows. High-selectivity columns make good indexes; the planner may ignore a low-selectivity one. |
| **Query plan** | The steps the database chooses to run a query. `EXPLAIN ANALYZE` shows it and is the first tool for slow queries. |

## Joins and query essentials

| Term | What it means |
|---|---|
| **`INNER JOIN`** | Returns only rows matching on both sides. The default choice when unmatched rows should be dropped. |
| **`LEFT JOIN`** | Keeps every left row, with `NULL` for missing right values. Finds items with or without related records. |
| **`FULL OUTER JOIN`** | Keeps all rows from both sides, matched where possible. Useful for reconciling two data sets. |
| **`CROSS JOIN`** | Every combination of rows from both tables. Useful for generating grids, but accidental ones explode row counts. |
| **Semi join** | Returns left rows having a match, without duplicating them. Usually written with `EXISTS` or `IN`. |
| **Anti join** | Returns left rows with no match. Prefer `NOT EXISTS`, because `NOT IN` misbehaves when `NULL` values are present. |
| **Nested loop join** | For each outer row, look up matches in the inner table. Efficient for small outer sets with an inner index. |
| **Hash join** | Builds a hash table from one input and probes it with the other. Efficient for large, unsorted equality joins. |
| **Merge join** | Walks two inputs sorted on the join key. Efficient when both are already sorted or indexed. |
| **CTE** (Common Table Expression) | Named subquery introduced with `WITH`. Improves readability, and the recursive form walks hierarchies. |
| **`WHERE` vs `HAVING`** | `WHERE` filters rows before grouping, and `HAVING` filters groups after. Filter early with `WHERE` where possible. |
| **Three-valued logic** | SQL comparisons return true, false or unknown. `x = NULL` is never true, so use `IS NULL`. |
| **Materialised view** | Stored query result refreshed on demand or a schedule. Speeds expensive reads, at the cost of staleness. |

```text
Logical order: FROM/JOIN -> WHERE -> GROUP BY -> HAVING -> window functions -> SELECT -> DISTINCT -> ORDER BY -> LIMIT
```

## Normal forms

| Term | What it means |
|---|---|
| **Normalisation** | Structuring tables to reduce redundancy. Prevents update, insert and delete anomalies, often at the cost of more joins. |
| **Functional dependency** | One set of columns determines another, such as `user_id` determining `email`. The basis for reasoning about normal forms. |
| **1NF** (First Normal Form) | Columns hold atomic values, with no repeating groups. Avoids comma-separated lists that are hard to query. |
| **2NF** (Second Normal Form) | 1NF, and non-key columns depend on the whole key. Matters with composite keys, where partial dependencies cause redundancy. |
| **3NF** (Third Normal Form) | 2NF, with no non-key column depending on another non-key column. A sensible target for most transactional schemas. |
| **BCNF** (Boyce-Codd Normal Form) | Every determinant must be a candidate key. A stricter 3NF that removes rarer anomalies. |
| **Denormalisation** | Deliberately duplicating data to speed reads. Useful for read-heavy paths, but the application must keep copies in step. |

## MVCC and storage internals

| Term | What it means |
|---|---|
| **MVCC** (Multi-Version Concurrency Control) | Keeps several row versions, so each transaction sees its own snapshot. Readers and writers mostly avoid blocking each other. |
| **Vacuum** | PostgreSQL process reclaiming dead row versions and freezing old rows. Long-running transactions hold it back, causing bloat. |
| **WAL** (Write-Ahead Log) | Changes are recorded in a durable log before data pages. Enables crash recovery, replication and point-in-time restores. |
| **Checkpoint** | Flushes dirty pages to disk and marks a recovery start point. Shorter recovery, traded against I/O spikes. |
| **`fsync`** | System call forcing buffered data onto stable storage. Commit durability depends on it, and it largely sets commit latency. |
| **PITR** (Point-In-Time Recovery) | Restores a base backup and replays logs to a chosen moment. Recovers from mistakes such as an accidental delete. |

## Replication

| Term | What it means |
|---|---|
| **Leader-follower replication** | One node takes writes and streams them to read replicas. Simple to reason about and scales reads, not writes. |
| **Synchronous replication** | Commit waits for replica acknowledgement. Avoids data loss on failover, but adds latency and availability risk. |
| **Asynchronous replication** | Commit returns before replicas apply it. Fast, but recent writes can be lost when failing over. |
| **Replication lag** | How far a replica trails the leader. Causes stale reads, so route read-your-writes traffic carefully. |
| **Physical replication** | Copies low-level storage changes, such as WAL bytes. Produces an exact copy but usually needs matching major versions. |
| **Logical replication** | Sends row-level changes, allowing partial copies and version differences. Useful for migrations and feeding other systems. |
| **Multi-leader replication** | Several nodes accept writes and exchange them. Helps multi-region writes, but conflicts must be resolved. |
| **Leaderless replication** | Clients write to several replicas and read using quorums. High availability, with weaker consistency unless carefully configured. |
| **Failover** | Promoting a replica when the leader dies. Risks data loss and split brain unless fencing and lag checks are in place. |
| **CDC** (Change Data Capture) | Streams committed changes, typically by reading the log. Feeds caches, search indexes and warehouses without dual writes. |

## Partitioning and sharding

| Term | What it means |
|---|---|
| **Partitioning** | Splitting a table into smaller pieces, usually within one database. Eases archival and speeds queries through partition pruning. |
| **Sharding** | Partitioning across multiple database servers. Scales writes and storage, but complicates joins, transactions and operations. |
| **Range partitioning** | Splits by key range, often by date. Supports efficient range scans and dropping old data, but recent ranges can run hot. |
| **Hash partitioning** | Splits by hash of the key for even distribution. Loses efficient range scans across the key. |
| **Shard key** | Column that decides data placement. The most important sharding decision, because changing it later is expensive. |
| **Hot partition** | One partition receives disproportionate traffic. Limits scaling; a better key, salting or splitting addresses it. |
| **Resharding** | Moving data to change the shard layout. Risky online work, made easier by consistent hashing or many logical shards. |
| **Scatter-gather query** | Sends a query to all shards and merges the results. Slower and less reliable, so design queries to include the shard key. |

## Window functions

| Term | What it means |
|---|---|
| **Window function** | Calculates across related rows while keeping each row in the output. Handles rankings and running totals without self joins. |
| **`PARTITION BY`** | Divides rows into groups for a window calculation. Unlike `GROUP BY`, it does not collapse them. |
| **`ROW_NUMBER()`** | Assigns a unique sequence within each partition. The standard tool for deduplication and "latest per group". |
| **`RANK()`** | Tied rows share a rank and leave gaps after, such as 1, 1, 3. |
| **`DENSE_RANK()`** | Tied rows share a rank with no gaps, such as 1, 1, 2. Suits "Nth highest" queries. |
| **`LAG()` and `LEAD()`** | Read a value from the previous or next row. Useful for period-over-period changes and detecting gaps. |
| **Window frame** | The row range for a calculation, for example `ROWS BETWEEN 6 PRECEDING AND CURRENT ROW`. Drives moving averages and running totals. |

```sql
-- Latest order per customer, plus a running total of spend
SELECT *
FROM (
  SELECT customer_id, order_id, amount,
         ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY created_at DESC) AS rn,
         SUM(amount)  OVER (PARTITION BY customer_id ORDER BY created_at)      AS running_total
  FROM orders
) ranked
WHERE rn = 1;
```

## OLTP and OLAP

| Term | What it means |
|---|---|
| **OLTP** (Online Transaction Processing) | Many short reads and writes on a few rows. Powers application back ends, where low latency and integrity matter most. |
| **OLAP** (Online Analytical Processing) | Large scans and aggregations across many rows. Powers reporting and analytics, where throughput on big queries matters most. |
| **Column store** | Keeps each column together on disk, unlike a row store. Compresses well and speeds aggregates touching few columns. |
| **Star schema** | Central fact table joined to denormalised dimension tables. A simple, fast shape for analytical queries. |
| **Snowflake schema** | Star schema with normalised dimensions. Saves some storage but needs more joins. |
| **Fact and dimension tables** | Facts store measurable events such as sales; dimensions store descriptive context used to filter and group them. |

| OLTP vs OLAP | OLTP | OLAP |
|---|---|---|
| Workload | Short transactions | Long analytical scans |
| Rows per query | Few | Millions or more |
| Storage layout | Usually row store | Usually column store |
| Schema | Normalised | Star or denormalised |
| Priority | Latency, integrity | Query throughput on large data |

## NoSQL families

| Term | What it means |
|---|---|
| **NoSQL** (Not Only Structured Query Language) | Non-relational databases trading some relational features for flexible models or horizontal scale. Choose by access pattern, not fashion. |
| **BASE** (Basically Available, Soft state, Eventual consistency) | Design approach favouring availability over immediate consistency. Often contrasted with ACID, although many NoSQL systems now offer transactions. |
| **Key-value store** | Fetches opaque values by key, as in Redis or DynamoDB. Extremely fast and simple, with limited querying. |
| **Document store** | Stores JSON-like documents, as in MongoDB. Flexible schema suits data read and written as a whole object. |
| **Wide-column store** | Rows grouped by partition key, sorted by clustering key, as in Cassandra. Scales writes well when queries are known upfront. |
| **Graph database** | Stores nodes and edges, as in Neo4j. Efficient for multi-hop relationship queries that need many SQL joins. |
| **Time-series database** | Optimised for timestamped appends and range aggregates. Offers downsampling and retention, useful for metrics and sensor data. |
| **Vector database** | Indexes embeddings for approximate nearest-neighbour search. Used for semantic search and retrieval in AI applications. |
| **Tunable consistency** | Client picks how many replicas must respond per operation. Lets each query trade consistency against latency and availability. |

| Family | Best for | Weak at | Examples |
|---|---|---|---|
| Key-value | Caches, sessions, simple lookups | Ad hoc queries | Redis, DynamoDB |
| Document | Nested, varied objects | Cross-document relationships | MongoDB, Couchbase |
| Wide-column | Very high write volume by known keys | Ad hoc queries, joins | Cassandra, HBase |
| Graph | Relationship traversal | Bulk analytics over all data | Neo4j |
| Time-series | Metrics, sensor data | General updates, joins | InfluxDB, TimescaleDB |
