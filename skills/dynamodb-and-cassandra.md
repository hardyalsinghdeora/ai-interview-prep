# DynamoDB & Cassandra

Wide-column and key-value stores scale by forcing you to design around queries up front, so interviewers use them to test whether you can model from access patterns and whether you know when the trade is a bad one. Staff/Principal backend and Software Architect loops ask most, usually as "how would this table look at a billion rows?", and solutions roles need a clear answer on when not to use them.

> Related: [System design](../topics/03-system-design/README.md) · [Large-scale distributed systems](large-scale-distributed-systems.md) · [AWS](aws.md) · [PostgreSQL & data modelling](postgresql-and-data-modelling.md)

---

## A. Fundamentals — must answer crisply
- [ ] What do the partition key and sort key (clustering key in Cassandra) each do? How do they determine where data lives and how it can be queried?
- [ ] Why do these databases require you to model from access patterns first, and how does that differ from relational modelling?
- [ ] DynamoDB `Query` vs. `Scan`: what does each cost, and why is a `Scan` with a filter still expensive?
- [ ] What is a Global Secondary Index in DynamoDB, how does it differ from a Local Secondary Index, and what consistency does each offer? *(verify)*
- [ ] DynamoDB eventually consistent vs. strongly consistent reads: what is the difference in behaviour and cost? *(verify)*
- [ ] Explain Cassandra's replication factor and consistency levels `ONE`, `QUORUM` and `LOCAL_QUORUM`. When is `R + W > N` useful?
- [ ] What makes Cassandra leaderless, and what do the coordinator and token ring do?
- [ ] What is a tombstone, and why do deletes create data rather than remove it?
- [ ] Name three kinds of workload where you would not use DynamoDB or Cassandra, and say why.

## B. Working depth
- [ ] What is single-table design, what problem does it solve, and how do you overload keys to serve several entity types?
- [ ] Single-table vs. multi-table design: when does the single-table approach become a burden on analytics, evolution or the team?
- [ ] How do you write conditional updates and optimistic locking with `ConditionExpression` in DynamoDB?
- [ ] DynamoDB transactions: what do they guarantee, what do they cost, and what limits apply? *(verify)*
- [ ] On-demand vs. provisioned capacity, RCU and WCU calculation, and how autoscaling behaves during a sudden spike. *(verify)*
- [ ] Sparse GSIs, GSI overloading and back-pressure: how can a slow GSI throttle writes to the base table? *(verify)*
- [ ] DynamoDB Streams and TTL: how do you use them together, and what timing or ordering promises do you actually get? *(verify)*
- [ ] Cassandra write path: commit log, memtable and SSTable. Why are writes fast, and why can reads be slower?
- [ ] Cassandra's read path: Bloom filters, partition indexes and how the number of SSTables touched affects latency.
- [ ] Cassandra lightweight transactions: how do they work, and why should they be used sparingly? *(verify)*
- [ ] Why do Cassandra secondary indexes and materialised views have a poor reputation, and what would you do instead? *(verify)*

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Hot partitions: how do you detect them in each database, how far does adaptive capacity help, and how do write sharding and bucketing fix them? *(verify)*
- [ ] Size-tiered vs. leveled vs. time-window compaction: which suits which workload, and what are the read, write and space amplification trade-offs? *(verify)*
- [ ] Tombstones in production: explain `gc_grace_seconds`, zombie data and why queue-like tables cause read timeouts.
- [ ] Hinted handoff, read repair and anti-entropy repair: what does each fix, and what happens when repair is not run on time? *(verify)*
- [ ] Why can Cassandra's last-write-wins lose updates, and what role do client clocks and timestamps play?
- [ ] Multi-region: DynamoDB Global Tables vs. Cassandra multi-datacentre replication. What happens on conflicting writes and during a region loss? *(verify)*
- [ ] Partition sizing in Cassandra: how large is too large, and how do you bucket time-series data? *(verify)*
- [ ] How do you run new access patterns on an existing table: add a GSI, backfill, or build a new table and migrate?
- [ ] Getting data out for analytics: DynamoDB export, Streams or CDC, and Spark on Cassandra. How do you avoid hurting production traffic? *(verify)*
- [ ] Compare the cost of DynamoDB, managed Cassandra-compatible services and self-managed clusters, including the staff time involved. *(verify)*
- [ ] Argue against DynamoDB for a young product whose queries keep changing. When would PostgreSQL be the better starting point, and what would trigger a later move?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Model a multi-tenant order system in DynamoDB supporting: get order, list a customer's orders by date, list orders by status and find orders by product.
- [ ] Design a time-series store for 2 million IoT devices in Cassandra with 90-day retention and "last 24 hours per device" queries.
- [ ] DynamoDB is throttling at well under the table's provisioned capacity. Walk through the diagnosis and the fix.
- [ ] Cassandra read timeouts are growing on one table only, and logs mention tombstone warnings. What happened, and how do you recover?
- [ ] Design an idempotency and deduplication store for a payments API on DynamoDB, including TTL, conditional writes and retries.
- [ ] A team proposes Cassandra for a system needing ad hoc reporting and multi-row invariants. Make the case against it and propose an alternative.

## E. Hands-on checks (things to be able to DO)
- [ ] Write out an access-pattern table, then a single-table key design with one GSI that serves all of the patterns.
- [ ] Use DynamoDB Local to write a conditional update and a transaction, and show the failure when a condition is not met.
- [ ] Calculate RCU and WCU from an item size and a request rate, then estimate a monthly bill. *(verify)*
- [ ] Run a local Cassandra cluster, stop a node and compare writes and reads at `ONE` and `QUORUM`, using `nodetool` and `cqlsh` tracing.
- [ ] Generate tombstones on purpose, measure read latency, then fix the table with better modelling and a suitable compaction strategy.

---

**Good answers show:**
- An access-pattern table written first, with keys and indexes derived from it.
- Clear thinking on partition distribution, and on what becomes hot as the business grows.
- Precise consistency language: quorum arithmetic, GSI lag, last-write-wins and what a transaction really covers.
- Operational awareness of compaction, tombstones and repair for Cassandra, and of capacity and throttling for DynamoDB.
- Cost modelling tied to request rates and item sizes, including staff time for self-managed clusters.
- Willingness to say no when queries are ad hoc, invariants span many rows or the product is still finding its shape.

**Prove it:** Model the same order-history workload in DynamoDB and Cassandra: write the access-pattern table, the key design and a load generator with a deliberately hot customer. Show the throttling or latency, fix it with write sharding or bucketing, add a new access pattern with a backfill, and publish the latency, cost estimate and a one-page decision on when you would use PostgreSQL instead.
