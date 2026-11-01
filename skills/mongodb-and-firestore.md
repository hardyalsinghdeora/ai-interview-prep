# MongoDB & Firestore

Document databases reward modelling around access patterns and punish teams that treat them as schemaless relational stores, so interviewers probe how you shape documents, choose keys and reason about consistency. Senior backend and solutions roles ask most, and Forward Deployed Engineers regularly inherit customer systems built on MongoDB or on Firestore behind a mobile or web app.

> Related: [Data engineering & SQL](../topics/07-data-and-sql/README.md) · [System design](../topics/03-system-design/README.md) · [PostgreSQL & data modelling](postgresql-and-data-modelling.md) · [Google Cloud (GCP)](google-cloud-gcp.md)

---

## A. Fundamentals — must answer crisply
- [ ] When is a document database a better fit than a relational one, and when is it clearly worse?
- [ ] Embedding vs. referencing: what rules do you use to decide, and how do read and write patterns drive the answer?
- [ ] What is a MongoDB replica set? Explain primary, secondary, arbiter and how an election works.
- [ ] Single-field, compound, multikey, partial, sparse and TTL indexes: give one real use for each.
- [ ] What is the ESR (equality, sort, range) guideline for compound index order, and why does it work?
- [ ] Explain read concern (`local`, `majority`, `snapshot`) and write concern (`w: 1`, `w: "majority"`, `j: true`) in plain language.
- [ ] What is `readPreference`, and what can go wrong when you read from secondaries?
- [ ] Explain Firestore's data model: documents, collections, subcollections and references. How does it differ from MongoDB's?
- [ ] What are Firestore's best-known limits on document size, write rate and transactions, and why do they exist? *(verify)*

## B. Working depth
- [ ] What is the document size limit in MongoDB, and how do you handle unbounded arrays (bucket, subset and outlier patterns)? *(verify)*
- [ ] Walk through an aggregation pipeline with `$match`, `$group`, `$lookup`, `$unwind` and `$project`. Which stages can use indexes, and where should `$match` go?
- [ ] How do you read `explain("executionStats")`? What do `COLLSCAN`, `IXSCAN`, `totalDocsExamined` and `nReturned` tell you?
- [ ] What makes a query covered by an index, and when is it worth designing for that?
- [ ] Multi-document transactions: how do they work, what limits and costs do they carry, and why does good document design often avoid them? *(verify)*
- [ ] What is schema validation in MongoDB, and how do you evolve a document's shape with lazy migration or the schema versioning pattern?
- [ ] What are change streams, what do they depend on, and what happens if a consumer falls behind the oplog window? *(verify)*
- [ ] Firestore queries: why does every query need an index, when do you need a composite index, and which query shapes are limited or unsupported? *(verify)*
- [ ] Firestore transactions vs. batched writes: how do server SDKs and mobile or web SDKs differ in their locking and retry behaviour? *(verify)*
- [ ] What do Firestore security rules do, and why are they not a replacement for server-side validation of business logic?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] How do you pick a MongoDB shard key? Discuss cardinality, frequency, monotonicity and which queries become scatter-gather.
- [ ] Ranged vs. hashed sharding, and what is zone sharding for? What can you change about a shard key after the fact? *(verify)*
- [ ] Failover and rollback: what can be lost with `w: 1`, and what do `w: "majority"` and retryable writes change?
- [ ] What does `causal consistency` give you in a session, and when does a read-your-own-writes guarantee still fail?
- [ ] Why do monotonically increasing values such as timestamps cause write hot spots in both MongoDB and Firestore? How do you avoid them?
- [ ] Firestore's hot-document problem: how do you keep a counter or popular aggregate correct under sustained writes (distributed counters, buffered aggregation)? *(verify)*
- [ ] How does Firestore pricing per read, write and delete change how you model data, and how do listeners and fan-out create surprise bills? *(verify)*
- [ ] Denormalisation in document stores: how do you keep copies in step, and how do you decide what staleness is acceptable?
- [ ] Multi-tenant design: shared collections with a tenant key vs. database per tenant vs. sharding on tenant. How do you handle one very large tenant?
- [ ] Firestore Native vs. Datastore mode, multi-region vs. regional, and the newer MongoDB-compatible options. What would you check before choosing? *(verify)*
- [ ] Atlas vs. self-managed MongoDB, licensing (SSPL) and API-compatible alternatives such as DocumentDB: what differences matter in practice? *(verify)*

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Model an e-commerce order with line items, shipment events and customer details in MongoDB. What do you embed and what do you copy, and how do you keep it correct?
- [ ] A MongoDB query that took 20 ms now takes 4 s as the collection reached 500 million documents. Walk through your diagnosis.
- [ ] A sharded cluster has one shard at 90% of the traffic. How do you find out why, and what are your options for a fix?
- [ ] Design a chat application on Firestore with rooms, read receipts and unread counts, keeping within its write limits and a reasonable bill.
- [ ] A Firestore-backed app's monthly bill has jumped tenfold with flat usage. How do you find the cause and bring the bill down?
- [ ] Migrate a customer's relational schema to MongoDB, or argue that it should move the other way. What decides the direction, and how do you cut over safely?
- [ ] After a failover, customers report that recently confirmed orders have disappeared. Explain how that can happen and what you would change.

## E. Hands-on checks (things to be able to DO)
- [ ] Write an aggregation pipeline that produces a monthly revenue-by-customer report, then index it and compare `explain` output.
- [ ] Set up a local three-node replica set, kill the primary and show what happens to writes at `w: 1` and `w: "majority"`.
- [ ] Build a change-stream consumer with a stored resume token, and show it recovering after a restart.
- [ ] Create a compound index using the ESR guideline and show the fall in `totalDocsExamined`.
- [ ] Use the Firestore emulator to write security rules with tests, a composite index and a distributed counter.

---

**Good answers show:**
- Modelling that starts from access patterns and states what gets denormalised and who keeps it correct.
- Index design backed by `explain` output rather than guesswork.
- Precise language on read and write concerns, including what can be lost during failover.
- Shard-key and hot-document reasoning: cardinality, monotonicity and write distribution.
- Limits and pricing treated as design inputs, especially for Firestore, and checked against current documentation.
- Honest comparison with a relational database, including when `jsonb` in PostgreSQL is the better call.

**Prove it:** Build the same order service twice, once on MongoDB and once on Firestore: documents for orders and events, a sales report, a live order-status feed and a high-traffic counter. Load test both, record the hot spots and the estimated monthly cost, kill a MongoDB primary at `w: 1` and at `w: "majority"`, and write up which store you would pick for which workload and why.
