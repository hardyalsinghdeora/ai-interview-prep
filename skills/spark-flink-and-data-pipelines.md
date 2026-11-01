# Spark, Flink & data pipelines

Data pipelines fail quietly: a job can succeed and still produce wrong numbers, so interviewers use this topic to test whether you can reason about execution, late data, reprocessing and trust in the output, not just write a transformation. Data-heavy senior backend, Software Architect and Forward Deployed Engineer roles ask most, because customer data rarely arrives clean, on time or in the shape you were promised.

> Related: [Data engineering & SQL](../topics/07-data-and-sql/README.md) · [Kafka & event streaming](kafka-and-event-streaming.md) · [PostgreSQL & data modelling](postgresql-and-data-modelling.md) · [Data migration & legacy modernisation](data-migration-and-legacy-modernisation.md)

---

## A. Fundamentals — must answer crisply
- [ ] Batch vs. streaming: what actually differs, and how do you decide how fresh the data really needs to be?
- [ ] In Spark, what are the driver, executors, jobs, stages and tasks, and how do they relate?
- [ ] Transformations vs. actions, and narrow vs. wide dependencies: why does Spark evaluate lazily, and where do stage boundaries come from?
- [ ] What is a shuffle, and why is it usually the most expensive part of a Spark job?
- [ ] Event time vs. processing time: why does the difference matter, and what is a watermark?
- [ ] ETL vs. ELT: what changed to make ELT common, and when is it still the wrong choice?
- [ ] What is an Airflow DAG, and what does it mean for a task to be idempotent and safe to re-run?
- [ ] What does dbt do, and what does it not do?
- [ ] What problem do table formats such as Iceberg, Delta Lake and Hudi solve on top of Parquet files in object storage?
- [ ] What is change data capture, and why is reading the database log usually better than polling on an `updated_at` column?

## B. Working depth
- [ ] What is data skew, how do you spot it in the Spark UI, and how do you fix it with salting, broadcast joins or adaptive query execution? *(verify)*
- [ ] Broadcast vs. sort-merge vs. shuffle-hash joins: how does Spark choose, and when would you override it?
- [ ] `repartition` vs. `coalesce`, and how does the small-files problem arise from a poorly partitioned write?
- [ ] Spark executor memory: how is it divided, and how do you tell a memory-pressure failure from a skew failure?
- [ ] Flink keyed state and checkpoints: what is stored where, and how does a job recover after a task failure? *(verify)*
- [ ] Tumbling, sliding and session windows: give one real use for each, and explain how allowed lateness and side outputs handle late events.
- [ ] Airflow: what do schedule interval, logical date, catchup, sensors and task retries each do, and why does the logical date confuse people? *(verify)*
- [ ] dbt: when would you use view, table, incremental and snapshot materialisations, and what goes wrong with a poorly chosen incremental key?
- [ ] Partitioning vs. clustering or Z-ordering in a lakehouse table: how do they affect query cost, and how do you pick the columns? *(verify)*
- [ ] What should a data quality check cover (freshness, volume, schema, uniqueness, referential integrity), and where in the pipeline should it block?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Spark Structured Streaming vs. Flink: micro-batch vs. continuous processing, state handling and latency. When does the difference matter? *(verify)*
- [ ] How does Flink provide exactly-once state and exactly-once output to a sink, and what must the sink support for that to hold?
- [ ] Flink state growth: how do you choose a state backend, set TTLs and handle a job whose state has grown to terabytes? *(verify)*
- [ ] A stateful streaming job needs a logic change. How do you use savepoints, and which changes cannot be restored from existing state? *(verify)*
- [ ] Watermarks stall because one idle partition has stopped sending events. What happens downstream, and how do you handle it?
- [ ] Lambda vs. Kappa architecture: what does maintaining two code paths really cost, and when is replay from a log impractical?
- [ ] Iceberg vs. Delta Lake vs. Hudi: compare metadata, concurrent writes, upserts and engine support. *(verify)*
- [ ] Copy-on-write vs. merge-on-read tables: how do they affect write cost, read latency and the compaction you must run?
- [ ] CDC failure modes: replication slots filling the disk, schema changes, initial snapshots, deletes and out-of-order updates. *(verify)*
- [ ] How do you reduce the cost of a pipeline estate: spot instances, autoscaling, file layout, warehouse vs. Spark compute, and retiring tables nobody reads? *(verify)*

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] A nightly Spark job that took one hour now takes six, and one task out of 2,000 runs for hours. Walk through the diagnosis and the fix.
- [ ] Revenue in the dashboard is 3% lower than in the billing system. How do you find out where the numbers diverge, and how do you stop it recurring?
- [ ] A bug corrupted three months of a derived table. Design the backfill so that it does not break downstream users or overload the source systems.
- [ ] Design a pipeline from production PostgreSQL to a lakehouse with CDC, schema evolution, deletes for privacy requests and a daily reconciliation.
- [ ] Design real-time fraud or usage aggregation with one-minute windows where events can arrive up to an hour late.
- [ ] A customer sends messy CSV files by SFTP each night, and the schema sometimes changes without warning. Design ingestion that fails safely and tells someone useful.
- [ ] A team wants to replace a working daily batch with streaming. What questions do you ask before agreeing?

## E. Hands-on checks (things to be able to DO)
- [ ] Read a Spark physical plan and the Spark UI, then identify the shuffle, the skewed stage and the join strategy.
- [ ] Write a PySpark job that fixes a skewed join, and show the before and after run times.
- [ ] Write an Airflow DAG with retries, idempotent daily partitions and a working backfill over a past date range.
- [ ] Build a small dbt project with an incremental model, tests and a snapshot, and show a failing test blocking a downstream model.
- [ ] Run a Flink job with event-time windows and a watermark, feed it late events, and take and restore a savepoint.
- [ ] Create an Iceberg or Delta table, perform an upsert, query an earlier snapshot, and run compaction.

---

**Good answers show:**
- Reasoning from the execution model: where data moves, what gets shuffled, and what state must be kept.
- Precise language on time: event time, watermarks and late data, with an explicit decision about what happens to late records.
- Pipelines built for re-running: idempotent writes, partition overwrite and a backfill plan from the first version.
- Data quality treated as a production concern, with checks, reconciliation against a source of truth, lineage and clear owners.
- Justified freshness: streaming chosen because the business needs it, not because it sounds more modern.
- Cost awareness across compute, storage and file layout, with evidence from query plans rather than guesses.

**Prove it:** Build a small order-analytics pipeline: CDC from PostgreSQL into Kafka, a Flink or Spark job writing to an Iceberg table, dbt models on top, and Airflow orchestrating daily reconciliation against the source. Inject late events, a schema change and a deliberate bug, then run a backfill and write up what the checks caught, what they missed, and the cost and time of the recovery.
