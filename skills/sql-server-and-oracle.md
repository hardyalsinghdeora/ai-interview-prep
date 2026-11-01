# SQL Server & Oracle

Banks, insurers, retailers and governments still keep their most important data in SQL Server and Oracle, so experience with open-source databases only gets you part of the way. Interviewers want to see that you can read a plan, reason about locking, protect the data and speak honestly about licensing and exit costs. Senior backend, Data Platform, Database Reliability and Software Architect roles in enterprise organisations ask most, often framed as "this query got slow overnight, and the licence renewal is due: what do you do?".

> Related: [PostgreSQL & data modelling](postgresql-and-data-modelling.md) · [MySQL](mysql.md) · [Data migration & legacy modernisation](data-migration-and-legacy-modernisation.md) · [Data & SQL](../topics/07-data-and-sql/README.md) · [Databases & SQL cheat sheet](../cheatsheets/databases-and-sql.md)

---

## A. Fundamentals — must answer crisply
- [ ] What is an execution plan? Compare estimated and actual plans in SQL Server with `EXPLAIN PLAN` and `DBMS_XPLAN.DISPLAY_CURSOR` in Oracle.
- [ ] Clustered and non-clustered indexes in SQL Server vs. heap tables and index-organised tables in Oracle: how is the data physically stored?
- [ ] Index seek vs. scan, key lookups and covering indexes: how does column order in a composite index affect which queries can use it?
- [ ] Which isolation levels does each engine offer, and what is the default in each?
- [ ] Why do readers not block writers in Oracle, and how does that differ from SQL Server's default behaviour without Read Committed Snapshot Isolation?
- [ ] What are the main differences between T-SQL and PL/SQL (batches vs. blocks, error handling, packages)? What surprises people about empty strings in Oracle?
- [ ] Stored procedures, functions and triggers: what is each for, and when should you avoid them?
- [ ] What does partitioning give you, and what does it not give you? Compare how you define it in each engine.
- [ ] Full, bulk-logged and simple recovery models in SQL Server vs. `ARCHIVELOG` and `NOARCHIVELOG` in Oracle: what does each allow you to restore?
- [ ] Always On availability groups vs. failover cluster instances, and Oracle RAC vs. Data Guard: what problem does each solve? *(verify)*

## B. Working depth
- [ ] How do you read a plan: estimated vs. actual rows, nested loops vs. hash vs. merge joins, and the warnings worth acting on?
- [ ] How do statistics drive the optimiser, and how do you manage them with `UPDATE STATISTICS` and `DBMS_STATS`, including histograms?
- [ ] Parameter sniffing in SQL Server and bind peeking in Oracle: how do they cause unstable plans, and what are the fixes (`OPTION (RECOMPILE)`, `OPTIMIZE FOR`, Query Store forcing, SQL Plan Management)? *(verify)*
- [ ] Filtered and columnstore indexes in SQL Server vs. bitmap and function-based indexes in Oracle: when would you use each?
- [ ] How do you capture and analyse a deadlock in each engine (`xml_deadlock_report`, ORA-00060 trace files), and what are the usual fixes?
- [ ] What is lock escalation in SQL Server, why does Oracle not have it, and why is `NOLOCK` a poor fix for blocking?
- [ ] Row-by-row cursors vs. set-based SQL: when do you use `BULK COLLECT` and `FORALL` in PL/SQL, and what hazards come with `MERGE`?
- [ ] PL/SQL packages: what do specification and body separation, package state, dependency invalidation and ORA-04068 mean for deployments?
- [ ] Error handling in procedures: `TRY...CATCH`, `XACT_ABORT` and `@@TRANCOUNT` vs. `EXCEPTION` blocks, `RAISE_APPLICATION_ERROR` and autonomous transactions.
- [ ] Dynamic SQL with `sp_executesql` and `EXECUTE IMMEDIATE`: how do bind parameters affect injection risk, the plan cache and the shared pool?
- [ ] Partition elimination, local vs. global indexes, and using `SWITCH` or `EXCHANGE PARTITION` to load and archive data quickly.

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Wait statistics and Query Store vs. AWR, ASH and ADDM: where do you start, and which of these tools require extra licensing? *(verify)*
- [ ] Synchronous vs. asynchronous commit in Always On: how do you set RPO and RTO targets, use readable secondaries and handle failover from the application side? *(verify)*
- [ ] How does RAC use Cache Fusion, what causes global cache contention, and when will RAC not improve write scalability? *(verify)*
- [ ] Design a backup strategy with full, differential and log backups or RMAN incrementals, including corruption checks and regular restore tests.
- [ ] Tempdb contention and the version store in SQL Server vs. undo sizing and ORA-01555 in Oracle: how do long-running transactions hurt each?
- [ ] Compare the licensing models: per-core and Server + CAL for SQL Server, processor and named-user metrics for Oracle, and the cost of editions, options and packs. *(verify)*
- [ ] What licensing traps come with virtualisation, cloud hosting and passive standby servers, and how do you prepare for a vendor audit? *(verify)*
- [ ] Stored procedures vs. application logic: how do you weigh performance and data proximity against testing, deployment and vendor lock-in?
- [ ] What is hard about moving to PostgreSQL: packages, `NUMBER` and `DATE` mappings, empty strings, identifier case, hints, `VACUUM` and connection handling?
- [ ] How would you compare migration tools and compatibility layers such as `ora2pg`, AWS Schema Conversion Tool, AWS DMS and Babelfish? *(verify)*

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] A query that ran in 200 ms now takes 40 seconds with no code change. Walk through your investigation on each engine.
- [ ] A nightly batch job causes blocking and deadlocks that time out daytime users when it overruns. How do you diagnose it and redesign it?
- [ ] A 2 TB orders table needs seven-year retention and fast recent-order queries. Design the partitioning, indexes and archive process with minimal locking.
- [ ] Design for an RPO of 5 minutes and an RTO of 30 minutes across two regions, once on SQL Server and once on Oracle, and explain the cost difference. *(verify)*
- [ ] A bad `UPDATE` ran at 14:07 without a `WHERE` clause. Walk through recovery on each engine, including tail-log backup and Oracle Flashback options.
- [ ] Leadership asks you to cut database licensing by 40%. What options do you assess before recommending a migration, and how do you present the total cost? *(verify)*
- [ ] Plan the migration of an Oracle application with 3,000 PL/SQL packages to PostgreSQL: assessment, conversion, data sync, testing, cutover and rollback.

## E. Hands-on checks (things to be able to DO)
- [ ] Capture an actual plan for a slow join, add the right index, and compare logical reads with `SET STATISTICS IO` or `DBMS_XPLAN`.
- [ ] Reproduce a deadlock with two sessions, capture the evidence, and fix it by changing the access order.
- [ ] Write the same upsert procedure in T-SQL and PL/SQL with error handling and explicit transaction control.
- [ ] Partition a table by month, show partition elimination in the plan, and swap out the oldest partition.
- [ ] Take full and log backups or an RMAN backup, restore to a chosen point in time, and record how long it took.
- [ ] Convert a small PL/SQL package to PL/pgSQL with `ora2pg`, then compare results between the two databases with automated tests.

---

**Good answers show:**
- Plan reading as a daily skill: comparing estimated with actual rows before reaching for an index or a hint.
- A precise understanding of how each engine's concurrency model differs, not an assumption that all relational databases behave alike.
- Procedural code judged on its merits, with attention to set-based logic, error handling and safe deployment.
- High availability and recovery tied to stated RPO and RTO targets, and backups proved by real restores.
- Licensing treated as an engineering constraint, with terms checked against current vendor documentation rather than quoted from memory.
- Migration realism: the code, the behavioural differences and the testing effort are all counted, not just the data copy.

**Prove it:** Take a small order-management schema with stored procedures on SQL Server Developer Edition or Oracle Database Free *(verify)*, and convert it to PostgreSQL. Compare plans and timings for five key queries, record every behaviour change you found, and write a one-page assessment of the effort, risks and cost of a full migration.
