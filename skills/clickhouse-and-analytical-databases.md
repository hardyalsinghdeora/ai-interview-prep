# ClickHouse & analytical databases

Sooner or later every product needs dashboards and reports that its transactional database cannot serve, so interviewers use analytical stores to test whether you understand columnar storage, data freshness and what a query really costs. Senior backend, data-heavy solutions and Forward Deployed Engineer roles ask most, because customers often want analytics over operational data without slowing down production.

> Related: [Data engineering & SQL](../topics/07-data-and-sql/README.md) · [Kafka & event streaming](kafka-and-event-streaming.md) · [Google Cloud (GCP)](google-cloud-gcp.md) · [PostgreSQL & data modelling](postgresql-and-data-modelling.md)

---

## A. Fundamentals — must answer crisply
- [ ] OLTP vs. OLAP: how do the workloads differ, and why does a query that scans 1% of the columns favour a column store?
- [ ] How does columnar storage work, and why does it compress so well and make aggregations so fast?
- [ ] Why are single-row updates and point lookups generally poor on analytical databases?
- [ ] What is a MergeTree table in ClickHouse? Explain parts, background merges and why they matter.
- [ ] What do `ORDER BY` and `PRIMARY KEY` mean in ClickHouse, and how is a sparse primary index different from a B-tree?
- [ ] What is `PARTITION BY` for in ClickHouse, and why is it not the main tool for query speed?
- [ ] Star schema vs. wide denormalised table: when would you pick each?
- [ ] BigQuery and Snowflake at a high level: how do they separate storage from compute, and what do you pay for? *(verify)*
- [ ] When is a read replica of PostgreSQL enough, and when do you genuinely need an analytical database?

## B. Working depth
- [ ] How do you choose `ORDER BY` columns in ClickHouse, and why does low-to-high cardinality ordering often help?
- [ ] What is a materialised view in ClickHouse, why is it an insert trigger rather than a stored query, and what are the pitfalls with backfills?
- [ ] ReplacingMergeTree, SummingMergeTree and AggregatingMergeTree: what does each do, and why are the results only eventually merged? *(verify)*
- [ ] How do you handle deduplication and updates in ClickHouse (`FINAL`, `argMax`, lightweight deletes, mutations)? *(verify)*
- [ ] Why do many small inserts cause trouble, and how do batching, async inserts and "too many parts" errors relate? *(verify)*
- [ ] Skip indexes and projections: what can they do that the primary key cannot, and what do they cost? *(verify)*
- [ ] How do codecs and low-cardinality column types affect compression and query speed? *(verify)*
- [ ] BigQuery: how do partitioning and clustering reduce scanned bytes, and how does slot-based capacity differ from on-demand pricing? *(verify)*
- [ ] Snowflake: virtual warehouses, micro-partitions, clustering keys and auto-suspend. How do they affect cost and concurrency? *(verify)*
- [ ] Feeding ClickHouse from Kafka: compare the Kafka table engine with a separate connector, including delivery guarantees and error handling. *(verify)*
- [ ] How does CDC map onto an append-oriented store? How do you represent updates and deletes from PostgreSQL or MySQL?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Replication and sharding in ClickHouse: distributed tables, replicated tables, Keeper and the failure modes of each. *(verify)*
- [ ] Self-hosted ClickHouse vs. ClickHouse Cloud vs. BigQuery vs. Snowflake: how would you decide on cost, concurrency, freshness and operations? *(verify)*
- [ ] Cost control: what are the biggest causes of runaway analytical spend, and which guardrails work (quotas, budgets, `maximum_bytes_billed`, auto-suspend, query review)? *(verify)*
- [ ] Join strategy in a column store: why can large joins hurt, and when would you denormalise, use dictionaries or pre-aggregate? *(verify)*
- [ ] Customer-facing analytics: how do you serve thousands of concurrent queries with low latency, and how is that different from an internal BI workload?
- [ ] Multi-tenant analytics: shared tables with tenant first in `ORDER BY` vs. per-tenant tables or databases, including row-level security and noisy neighbours.
- [ ] Data freshness vs. cost: batch vs. micro-batch vs. streaming, and how you agree on the required freshness with the business.
- [ ] Late-arriving data, out-of-order events and reprocessing: how do you correct pre-aggregated results without double counting?
- [ ] Schema evolution and long retention: how do you add columns, change an `ORDER BY` and tier old data to object storage? *(verify)*
- [ ] Open table formats such as Iceberg: what do they change about warehouse lock-in, and what do they not solve? *(verify)*

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Design product analytics for 5 billion events a day with sub-second dashboard queries and a 13-month retention period.
- [ ] Analytics queries are slowing down a production PostgreSQL database. Design the path to an analytical store, including backfill, CDC and cut-over of the dashboards.
- [ ] A BigQuery or Snowflake bill has tripled in a month. Walk through how you find the cause and bring it down without breaking reports. *(verify)*
- [ ] ClickHouse inserts are failing with "too many parts" during a traffic peak. What is happening, and how do you recover and prevent it?
- [ ] A dashboard total differs from the billing system by 0.3%. How do you trace the difference through the pipeline, and how would you find duplicates or late data?
- [ ] Add usage analytics to a multi-tenant SaaS product where each customer can only see its own data. Design the tables, the pre-aggregations and the access control.
- [ ] A customer already has Snowflake and wants real-time operational alerts as well. What would you recommend, and when would you add another system?

## E. Hands-on checks (things to be able to DO)
- [ ] Create a MergeTree table, load tens of millions of rows and compare two `ORDER BY` designs using `EXPLAIN` and `system.query_log`.
- [ ] Inspect `system.parts` to show parts and compression, then watch merges reduce the part count. *(verify)*
- [ ] Build a materialised view into an AggregatingMergeTree table and backfill it safely from existing data.
- [ ] Stream from a Kafka topic into ClickHouse, then show how duplicates are handled when you replay the topic.
- [ ] Use a dry run in BigQuery to estimate scanned bytes, then cut them down with partitioning and clustering. *(verify)*
- [ ] Run the same query on PostgreSQL and ClickHouse with the same data, and explain the difference in speed and storage size.

---

**Good answers show:**
- Reasoning from physical layout: columns, sorted parts, sparse indexes and bytes scanned.
- `ORDER BY` and pre-aggregation designed from the real query mix rather than copied from a schema.
- Clear statements about eventual merges, duplicates, late data and what they do to correctness.
- An explicit freshness requirement that drives the choice of batch, micro-batch or streaming.
- Cost treated as a design input, with guardrails, ownership and current pricing checked.
- Honest scoping: a read replica, a summary table or a warehouse the customer already has may be enough.

**Prove it:** Stream a PostgreSQL orders table through CDC and Kafka into ClickHouse, handling updates and deletes with a ReplacingMergeTree design, and add a materialised view behind a multi-tenant revenue dashboard. Replay the topic, inject late data and reconcile the totals against PostgreSQL. Publish the query latency, the compression ratio, the data freshness and a cost comparison with running the same workload on BigQuery or Snowflake.
