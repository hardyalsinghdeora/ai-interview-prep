# MySQL

MySQL runs a huge share of the world's web applications, often at a scale that exposes every detail of InnoDB, replication and schema change, so interviewers ask about it to see whether you understand what the engine is doing rather than only the SQL. Senior backend and Staff/Principal roles at large consumer companies go deepest, and Forward Deployed Engineers often meet it in the customer systems they have to integrate with or migrate.

> Related: [Data engineering & SQL](../topics/07-data-and-sql/README.md) · [PostgreSQL & data modelling](postgresql-and-data-modelling.md) · [Data migration & legacy modernisation](data-migration-and-legacy-modernisation.md) · [Debugging & troubleshooting](../topics/12-debugging-troubleshooting/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] What is a clustered index in InnoDB, and how is a table's data physically stored?
- [ ] How does a secondary index lookup work in InnoDB, and why does the primary key size affect every secondary index?
- [ ] Why does a random UUID primary key hurt InnoDB write performance, and what would you use instead? *(verify)*
- [ ] What is a covering index, and what does `Using index` tell you in `EXPLAIN`?
- [ ] What is the leftmost prefix rule for composite indexes? Give a query that can and one that cannot use an index.
- [ ] Explain the isolation levels, and state which one is InnoDB's default. *(verify)*
- [ ] What are the buffer pool, redo log and undo log each for?
- [ ] What is the binary log, and how does it differ from the redo log?
- [ ] What is replication lag, and what user-visible problem does it cause?

## B. Working depth
- [ ] How does MVCC work in InnoDB using undo logs and read views? When does a consistent read see an old row version?
- [ ] Record, gap and next-key locks: what does each prevent, and how does `REPEATABLE READ` use them to limit phantoms?
- [ ] How does a deadlock occur in InnoDB, how is it detected, and how should the application respond?
- [ ] How does an `UPDATE` run in InnoDB from start to commit? Include the buffer pool, undo, redo, binlog and two-phase commit.
- [ ] How do you read `EXPLAIN` and `EXPLAIN ANALYZE` output, including access type, `rows`, `filtered` and `Using filesort`? *(verify)*
- [ ] Statement-based vs. row-based vs. mixed binlog format: which risks and costs does each carry? *(verify)*
- [ ] What are GTIDs, and how do they make failover and re-pointing replicas easier than file and position?
- [ ] Asynchronous vs. semi-synchronous replication vs. Group Replication: what does each guarantee about losing a committed write? *(verify)*
- [ ] What do `innodb_flush_log_at_trx_commit` and `sync_binlog` control, and how do they trade durability for throughput? *(verify)*
- [ ] How do online DDL's `INSTANT`, `INPLACE` and `COPY` algorithms differ, and which locks can still block traffic? *(verify)*
- [ ] Range, list and hash partitioning: when does partitioning help, and what are the restrictions on keys and foreign keys? *(verify)*

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] `gh-ost` vs. `pt-online-schema-change` vs. native online DDL: how does each work, and what can go wrong on a very large, busy table? *(verify)*
- [ ] Why can a long-running transaction hurt the whole server? Discuss history list length, undo growth and metadata locks.
- [ ] Read-after-write on replicas: how do you handle it with primary pinning, GTID waits or session tokens?
- [ ] Failover design: how do you detect a failed primary, promote safely and avoid split-brain, with tools such as Orchestrator or a managed service? *(verify)*
- [ ] Parallel replication: how do multi-threaded replicas reduce lag, and what still causes a replica to fall behind? *(verify)*
- [ ] Vitess-style sharding: what do VTGate, VTTablet and VSchema do, how do you pick a sharding key, and how does resharding work online? *(verify)*
- [ ] What gets harder after sharding: cross-shard queries and transactions, unique constraints, foreign keys and schema change?
- [ ] Connection handling: why do thousands of connections hurt, and what do ProxySQL or a thread pool change? *(verify)*
- [ ] MySQL vs. PostgreSQL: compare MVCC and vacuum, clustered storage, replication, indexing features and extension ecosystems. How would you choose?
- [ ] Aurora MySQL, Cloud SQL and other managed services: what changes in the storage and replication model, and what do you still own? *(verify)*
- [ ] MySQL major-version upgrades and long-term support releases: how do you plan one, including optimiser plan changes and rollback? *(verify)*

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] You need to add a column and an index on a 3 TB table with 20,000 writes a second and no downtime. Walk through the plan and the abort criteria.
- [ ] A query was fast yesterday and is slow today with no deploy. How do you find out what changed, including statistics and the plan?
- [ ] The application reports deadlocks after a new batch job shipped. How do you read `SHOW ENGINE INNODB STATUS`, find the cause and fix it?
- [ ] A replica is six hours behind and users are complaining about stale data. How do you investigate and recover?
- [ ] A single MySQL primary is reaching its limits. Plan the journey through tuning, read replicas, functional splitting and sharding, and say what would trigger each step.
- [ ] Migrate a production MySQL database to PostgreSQL, or into a new region, with minimal downtime. How do you validate data and cut over?
- [ ] After a failover, some committed orders are missing and replicas disagree. Explain how that can happen and what you would change.

## E. Hands-on checks (things to be able to DO)
- [ ] Use two sessions to demonstrate a dirty read, a non-repeatable read and a gap-lock block at different isolation levels.
- [ ] Find the slowest queries using the slow query log, `pt-query-digest` or Performance Schema, then fix one with an index. *(verify)*
- [ ] Set up a primary and replica with GTIDs in Docker, break replication, repair it and promote the replica.
- [ ] Run `gh-ost` or an online DDL on a table under load, and measure the effect on latency and replica lag.
- [ ] Use `EXPLAIN ANALYZE` to compare a query with a covering index and without one, and explain the difference.
- [ ] Reproduce a deadlock on purpose, read the report from `SHOW ENGINE INNODB STATUS` and add a safe retry.

---

**Good answers show:**
- Reasoning from InnoDB's physical layout: clustered storage, secondary index lookups and page behaviour.
- Precise language about isolation, gap locks and MVCC, with examples rather than textbook definitions.
- Clear durability trade-offs across flush settings, binlog settings and replication modes.
- A safe plan for schema changes on large tables, including lag monitoring, throttling and abort criteria.
- A staged scaling story in which sharding comes last, with its costs named honestly.
- A balanced MySQL vs. PostgreSQL comparison, based on the workload and team rather than on preference.

**Prove it:** Run a small MySQL setup with a primary, two GTID replicas and a load generator. Add a column to a table of tens of millions of rows with `gh-ost` under load, force a failover and measure the writes lost at different durability settings. Then shard one table with Vitess or a simple application-level router, and write up the operational costs alongside a comparison with running the same workload on PostgreSQL.
