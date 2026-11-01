# Elasticsearch & OpenSearch

Search looks simple until relevance complaints, mapping explosions and a red cluster arrive, so interviewers use it to test whether you understand inverted indexes, shard sizing and how a search index stays in step with its source of truth. Senior backend and data-heavy Forward Deployed Engineer roles ask most, because customers often want search or log analytics added to a system that already has a primary database.

> Related: [System design](../topics/03-system-design/README.md) · [Data engineering & SQL](../topics/07-data-and-sql/README.md) · [Kafka & event streaming](kafka-and-event-streaming.md) · [Vector databases](vector-databases.md)

---

## A. Fundamentals — must answer crisply
- [ ] What is an inverted index, and why does it make full-text search fast where a B-tree `LIKE '%term%'` does not?
- [ ] Explain cluster, node, index, shard, replica and segment, and how they relate to a Lucene index.
- [ ] `text` vs. `keyword` fields: how is each indexed, and which would you use for filtering, sorting, aggregations and full-text search?
- [ ] What does an analyser do? Walk through character filters, the tokeniser and token filters using a real string.
- [ ] Query context vs. filter context: what is the difference for scoring and caching, and when should a clause go in `filter`?
- [ ] What do primary shards and replicas each give you, and which of the two can you change on a live index without a reindex?
- [ ] What do green, yellow and red cluster health mean? Which ones lose reads or writes?
- [ ] Why is Elasticsearch usually a secondary store rather than the system of record?
- [ ] Why was OpenSearch forked from Elasticsearch, and how did the licences of each change over time? *(verify)*

## B. Working depth
- [ ] Dynamic vs. explicit mappings: what goes wrong with dynamic mapping in production, and what do `dynamic: strict` and field limits protect against? *(verify)*
- [ ] Why can you not change a field's type in place, and what are your options when you need to?
- [ ] Explain the write path: translog, in-memory buffer, refresh, flush and segment merges. Why is search "near real time"?
- [ ] How does BM25 score a document? What do term frequency, inverse document frequency and field-length normalisation each contribute? *(verify)*
- [ ] How do you tune relevance with `multi_match`, field boosts, `function_score` and synonyms, without tuning blind?
- [ ] Index-time vs. search-time analysers: when do they need to differ, and how do you handle autocomplete with n-grams or a completion suggester?
- [ ] Pagination: why does deep `from`/`size` hurt, and when would you use `search_after` with a point-in-time or the scroll API? *(verify)*
- [ ] What are index aliases, and how do they support zero-downtime reindexing and rollback?
- [ ] Nested vs. flattened object fields vs. parent-child joins vs. denormalising: what does each cost at index and query time? *(verify)*
- [ ] What does the `_bulk` API do, how do you handle partial failures, and how do you pick a batch size?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] How do you size shards and choose a shard count? What goes wrong with too many small shards vs. too few large ones? *(verify)*
- [ ] Master-eligible, data, ingest and coordinating roles: how do you lay them out, and what does quorum protect against in a split-brain?
- [ ] What causes JVM heap pressure (fielddata, large aggregations, high cardinality), and how do circuit breakers and heap sizing guidance help? *(verify)*
- [ ] Time-based data: rollover, index lifecycle policies and hot/warm/cold tiers, including the OpenSearch equivalents. *(verify)*
- [ ] Multi-tenant indexing: shared index with routing vs. index per tenant vs. cluster per tenant. Compare noisy neighbours, mapping explosion, relevance statistics and deletion.
- [ ] Why can BM25 scores differ across shards, and when would small shards or uneven routing make this visible to users?
- [ ] How do you keep the index consistent with a primary database: dual writes vs. outbox vs. CDC? How do you handle ordering with external versioning?
- [ ] Elasticsearch vs. OpenSearch today: licences, API compatibility, client libraries, managed offerings and feature divergence. How would you choose? *(verify)*
- [ ] Hybrid search: how do you combine BM25 with vector search, and when would you keep vectors in a dedicated store instead? *(verify)*
- [ ] Why are deletes and updates expensive in Lucene, and how do merges, segment count and force-merge affect performance?
- [ ] When is Postgres full-text search, or a managed search product, enough? Argue against adding a search cluster.

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Design product search for a marketplace with 50 million items, typo tolerance, facets and per-seller boosting, synced from PostgreSQL.
- [ ] You need to change an analyser on a 2 TB live index. Walk through a zero-downtime reindex, including writes that arrive during the copy and the rollback plan.
- [ ] The cluster went red after a node failure. Walk through diagnosis with allocation explain and how you recover without losing data.
- [ ] Search p99 jumped from 80 ms to 2 s after a new dashboard shipped. How do you find the query responsible and fix it?
- [ ] Search results show stale or missing records compared with the primary database. How do you find the cause and build a reconciliation job?
- [ ] Design log analytics for 5 TB a day with 30-day hot retention, one-year cold retention and a fixed budget.
- [ ] A SaaS customer complains that relevance is poor for their domain vocabulary. How do you measure the problem and fix it for one tenant only?

## E. Hands-on checks (things to be able to DO)
- [ ] Write an explicit mapping with a custom analyser, then use `_analyze` to show the tokens it produces.
- [ ] Use `_explain` and the profile API to show why one document outranks another and where a query spends its time.
- [ ] Perform a reindex behind an alias with `_reindex`, then swap the alias atomically and roll back.
- [ ] Use `_cat/shards`, `_cluster/health` and `_cluster/allocation/explain` to diagnose an unassigned shard.
- [ ] Build a small judged query set and calculate nDCG or precision@k before and after a relevance change.
- [ ] Stream changes from PostgreSQL into an index through Debezium and Kafka, using external versioning to reject out-of-order updates.

---

**Good answers show:**
- Reasoning from the inverted index and segments, so that merges, refresh costs and mapping limits follow naturally.
- Mappings treated as a contract: explicit, versioned and changed through aliases and reindexing.
- Relevance treated as a measured discipline, with judged queries and offline metrics rather than boost guesswork.
- A clear sync design that names the source of truth, ordering, backfill and reconciliation.
- Capacity thinking: shard counts, heap, retention tiers and the cost of each tenant.
- Current, checked knowledge of the licence and feature differences, with honest scoping of when a search cluster is overkill.

**Prove it:** Build a product search service over a PostgreSQL catalogue: CDC through Kafka into OpenSearch or Elasticsearch, an explicit mapping with custom analysers, faceted search and autocomplete. Change the analyser with a zero-downtime alias swap under write load, score relevance before and after on a judged query set, and write up one failure you caused on purpose, such as an unassigned shard or a stale-sync gap.
