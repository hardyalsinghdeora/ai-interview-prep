# Data Engineering Cheatsheet

Terms for moving, storing, modelling and trusting analytical data, including pipelines, table formats, Spark, Airflow and dbt.

> Related: [Data and SQL questions](../topics/07-data-and-sql/README.md) · [PostgreSQL and data modelling skills](../skills/postgresql-and-data-modelling.md) · [Kafka and event streaming skills](../skills/kafka-and-event-streaming.md) · [Data migration and legacy modernisation skills](../skills/data-migration-and-legacy-modernisation.md) · [Databases and SQL](databases-and-sql.md) · [Messaging and streaming](messaging-and-streaming.md)

## Pipelines

| Term | What it means |
|---|---|
| **ETL** (Extract, Transform, Load) | Transforming data before loading it into the target; suits strict schemas or sensitive data that must be cleaned first. |
| **ELT** (Extract, Load, Transform) | Loading raw data first and transforming inside the warehouse; flexible and now the default on cloud platforms. |
| **Batch processing** | Handling data in scheduled groups; simple and cost-effective when results can be minutes or hours old. |
| **Stream processing** | Handling events continuously as they arrive; needed for low-latency decisions, but harder to operate. |
| **Idempotent pipeline** | A run that gives the same result however many times it executes; makes retries and backfills safe. |
| **Backfill** | Reprocessing historical data after a bug fix or new logic; pipelines must be designed to allow it safely. |
| **Late-arriving data** | Records that show up after their time window was processed; handled with watermarks or reprocessing. |
| **Watermark** | A marker estimating how far event time has progressed, deciding when a streaming window can close. |
| **Event time vs processing time** | When something happened versus when the system saw it; using the wrong one misplaces late data. |
| **Exactly-once semantics** | Each record affects results once, even with retries; in practice achieved through at-least-once delivery with idempotent writes. |
| **At-least-once semantics** | Records are never lost but may repeat; the common default, requiring consumers to handle duplicates. |
| **CDC** (Change Data Capture) | Streaming inserts, updates and deletes from a database's log; gives fresh data with little load on the source. |
| **Debezium** | A widely used open-source CDC tool that reads database logs and publishes changes, typically to Kafka. |
| **Reverse ETL** (Reverse Extract, Transform, Load) | Syncing modelled warehouse data back into operational tools such as CRMs, so teams can act on it. |
| **Schema drift** | Source structure changing unexpectedly, such as a renamed column; a leading cause of pipeline failures. |
| **Schema evolution** | Controlled changes to a table or message schema that keep existing readers and writers working. |
| **Dead-letter queue** | A place for records that repeatedly fail processing, so bad data can be inspected without blocking the pipeline. |
| **Entity resolution** | Identifying records in different systems that refer to the same real-world customer or product; essential when merging sources. |

### ETL vs ELT

| | ETL | ELT |
|---|---|---|
| Transform happens | Before loading, on separate compute | Inside the warehouse or lakehouse |
| Raw data kept | Not always | Yes, so logic can be re-run |
| Good for | Strict compliance, legacy systems | Cloud analytics, changing requirements |

## Storage and platforms

| Term | What it means |
|---|---|
| **OLTP** (Online Transaction Processing) | Systems handling many small reads and writes, such as orders; optimised for speed and consistency per record. |
| **OLAP** (Online Analytical Processing) | Systems for large aggregate queries across history; optimised for scanning columns rather than updating rows. |
| **Data warehouse** | A governed store of structured data modelled for analytics; fast SQL and trusted reporting, less suited to raw files. |
| **Data lake** | Cheap object storage holding raw data in any format; flexible, but without governance it becomes a data swamp. |
| **Lakehouse** | Warehouse features such as ACID transactions on lake storage through table formats; one platform for BI and ML. |
| **Data mart** | A smaller warehouse subset focused on one team or subject, such as finance reporting. |
| **Medallion architecture** | Layering data as bronze (raw), silver (cleaned) and gold (business-ready); makes quality stages clear. |
| **Object storage** | Scalable file-like storage such as Amazon S3; the cheap foundation of lakes and lakehouses. |
| **Columnar storage** | Storing values by column rather than row; analytical queries read less data and compress far better. |
| **Parquet** | An open columnar file format with compression and statistics; the default for analytical data on lakes. |
| **ORC** (Optimized Row Columnar) | A columnar file format from the Hadoop ecosystem, similar to Parquet; common with Hive. |
| **Avro** | A row-based format with an embedded schema; well suited to streaming messages and schema evolution. |
| **Table format** | A metadata layer turning lake files into tables with ACID commits, schema evolution and time travel. |
| **Apache Iceberg** | An open table format with snapshots, hidden partitioning and wide engine support, avoiding lock-in to one engine. |
| **Delta Lake** | An open table format built around a transaction log, closely associated with Spark and Databricks. |
| **Apache Hudi** (Hadoop Upserts Deletes and Incrementals) | An open table format focused on frequent upserts and incremental processing, useful for CDC-heavy data. |
| **Time travel** | Querying a table as it was at an earlier version; helps audits, debugging and recovering from bad writes. |
| **Partitioning** | Splitting a table by a column such as date, so queries skip irrelevant data; too many partitions cause overhead. |
| **Partition pruning** | The engine skipping partitions that a query filter rules out; the main performance benefit of partitioning. |
| **Bucketing** | Hashing rows into a fixed number of files by a key; speeds up joins on that key. |
| **Small files problem** | Too many tiny files slow reads and strain metadata; fixed with compaction. |
| **Compaction** | Merging small files into larger ones to restore read performance; a routine lakehouse maintenance job. |

### Warehouse vs lake vs lakehouse

| | Warehouse | Lake | Lakehouse |
|---|---|---|---|
| Data | Structured, modelled | Anything, raw | Anything, with table layer |
| Cost | Higher | Lowest | Low to medium |
| Strength | BI and governance | Flexibility and ML | Both, on open formats |
| Risk | Lock-in, poor fit for unstructured | Data swamp | Maintenance and maturity |

### File formats

| Format | Layout | Best for |
|---|---|---|
| CSV (Comma-Separated Values) | Row, text | Simple exchange, no types enforced |
| JSON (JavaScript Object Notation) | Row, text | Nested data and APIs, costly to scan |
| Avro | Row, binary | Streaming, schema evolution |
| Parquet or ORC | Column, binary | Analytics at scale |

## Spark

| Term | What it means |
|---|---|
| **Apache Spark** | A distributed engine for large-scale batch and streaming processing, running work in parallel across a cluster. |
| **Driver** | The Spark process that plans the job, schedules tasks and collects results; collecting too much causes out-of-memory errors. |
| **Executor** | A worker process that runs tasks and holds cached data; sized by memory and cores. |
| **DAG** (Directed Acyclic Graph) | A graph of steps with no cycles; Spark and Airflow both use it to represent execution order. |
| **Lazy evaluation** | Spark records transformations but runs nothing until an action, allowing it to optimise the whole plan. |
| **Transformation vs action** | Transformations such as filter define new datasets lazily; actions such as count trigger actual execution. |
| **Narrow vs wide transformation** | Narrow works within a partition; wide, such as groupBy, needs data from others and causes a shuffle. |
| **Shuffle** | Moving data across the network between executors; usually the most expensive part of a Spark job. |
| **Data skew** | A few keys hold far more data, so some tasks lag or fail; fix with salting or broadcast joins. |
| **Salting** | Adding a random suffix to skewed keys to spread them across partitions, then combining the results. |
| **Broadcast join** | Sending a small table to every executor, avoiding a shuffle; ideal for joining a large table to a small one. |
| **Structured Streaming** | Spark's streaming API, treating a stream as an ever-growing table processed in small increments. |

## Orchestration and transformation

| Term | What it means |
|---|---|
| **Orchestration** | Scheduling and coordinating pipeline steps with dependencies, retries and alerts; Airflow, Dagster and Prefect are examples. |
| **Apache Airflow** | A widely used orchestrator where workflows are defined in Python as DAGs and run on a schedule. |
| **Airflow task** | One unit of work in a DAG; should be idempotent, so retries are safe. |
| **Operator** | A reusable Airflow task template, such as running SQL or a Python function. |
| **Sensor** | A task that waits for a condition, such as a file arriving; poorly configured sensors can tie up worker slots. |
| **Catchup** | Airflow running all missed schedule intervals since a start date; useful for backfills, surprising if left on accidentally. |
| **XCom** (cross-communication) | Airflow's way to pass small values between tasks; large data should go through storage instead. |
| **dbt** (data build tool) | Turns SQL SELECT models into warehouse tables, adding dependencies, tests and documentation; the T in ELT. |
| **dbt model** | A SQL file defining one table or view; references to other models build the dependency graph. |
| **Incremental model** | A dbt model that processes only new or changed rows, reducing cost on large tables but needing careful late-data handling. |
| **dbt test** | A data assertion such as unique or not null that fails the run, catching bad data early. |
| **dbt snapshot** | Records how mutable source rows change over time; a standard way to build Type 2 history. |

## Data modelling

| Term | What it means |
|---|---|
| **Star schema** | A central fact table joined to denormalised dimension tables; simple and fast for BI queries. |
| **Snowflake schema** | A star schema with normalised dimensions; less redundancy, but more joins and complexity. |
| **Fact table** | Stores measurable events such as sales, with numbers and foreign keys; usually very large. |
| **Dimension table** | Stores descriptive context such as customer or product; used for filtering and grouping. |
| **Grain** | What one row in a fact table represents, such as one order line; define it first to avoid wrong totals. |
| **Surrogate key** | A system-generated key independent of source IDs; needed to track dimension history and merge sources. |
| **SCD** (Slowly Changing Dimension) | How a dimension records attribute changes over time; the choice decides whether history can be reported correctly. |
| **Semantic layer** | Central business definitions of metrics and entities, so every tool calculates revenue the same way. |
| **Ontology** | A model of business objects and relationships mapped to data, as in Palantir Foundry; lets applications work in business terms. |

### SCD types

| Type | Behaviour | History kept |
|---|---|---|
| Type 1 | Overwrite the old value | None |
| Type 2 | Add a new row with validity dates | Full |
| Type 3 | Add a column for the previous value | Limited |

## Data quality and governance

| Term | What it means |
|---|---|
| **Data contract** | An agreement between data producers and consumers on schema, meaning and quality; stops upstream changes silently breaking users. |
| **Data quality dimensions** | Common measures: accuracy, completeness, consistency, timeliness, uniqueness and validity; useful for defining what to check. |
| **Data freshness** | How recent the available data is compared with its source; often has its own SLA. |
| **Quarantine** | Moving failed records aside for review while good data continues; balances pipeline uptime and data correctness. |
| **Great Expectations** | An open-source framework for defining and running data validation checks in a pipeline. |
| **Data lineage** | Tracing where data came from and what depends on it; essential for impact analysis, debugging and audit. |
| **Data catalogue** | A searchable inventory of datasets with owners and definitions, helping people find and trust data. |
| **Data observability** | Monitoring freshness, volume, schema and distributions, to detect data problems before users do. |
| **Data masking** | Hiding sensitive values with altered forms in non-production or restricted views; protects PII while keeping data usable. |
| **Tokenisation** (data privacy) | Replacing sensitive values with non-sensitive tokens held in a secure vault; shrinks the systems handling real data. |
