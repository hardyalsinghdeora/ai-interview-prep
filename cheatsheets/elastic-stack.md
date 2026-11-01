# Elastic Stack Cheatsheet

Covers Elasticsearch internals and operations, the query DSL, and Kibana, Logstash and Beats, going deeper than the broader search notes in [search-and-caching.md](search-and-caching.md).

> Related: [search-and-caching.md](search-and-caching.md) · [Elasticsearch and OpenSearch skill](../skills/elasticsearch-and-opensearch.md) · [Vector databases skill](../skills/vector-databases.md) · [observability-and-reliability.md](observability-and-reliability.md) · [distributed-systems.md](distributed-systems.md) · [System design](../topics/03-system-design/README.md)

## Cluster and node roles

| Term | What it means |
|---|---|
| **Cluster** | Nodes sharing one name and cluster state, working together to store and search data. |
| **Cluster state** | Metadata about nodes, indices, mappings and shard locations; when it grows very large, cluster changes slow down. |
| **Master-eligible node** | A node that can be elected master and votes on cluster state changes; run at least three. |
| **Elected master** | The one node that publishes cluster state and allocates shards; it should not be overloaded by data work. |
| **Voting quorum** | A majority of master-eligible nodes must agree on changes, which prevents split-brain during a network partition. |
| **Data node** | Holds shards and runs indexing, search and aggregations; usually the most resource-hungry role. |
| **Data tier** | Node roles such as `data_hot`, `data_warm`, `data_cold` and `data_frozen`, which match hardware to data age. |
| **Coordinating node** | Receives a request, sends it to shards and merges results; every node does this, and it can be dedicated. |
| **Ingest node** | Runs ingest pipelines on documents before they are indexed. |
| **Cluster health** | Green means all shards are assigned, yellow means a replica is not, and red means a primary is not. |
| **Shard allocation awareness** | Spreads primary and replica copies across racks or zones, so one zone failure cannot lose all copies. |
| **Disk watermarks** | Disk-use thresholds of 85%, 90% and 95% by default, controlling allocation, then moving shards, then blocking writes *(verify)*. |
| **Circuit breaker** | Rejects requests likely to exhaust JVM heap, returning an error instead of crashing the node. |
| **JVM heap** (Java Virtual Machine heap) | Elasticsearch's working memory, auto-sized by default; keep below about 50% of RAM and 31 GB *(verify)*. |

## Index and shards

| Term | What it means |
|---|---|
| **Index** | A logical namespace of documents with shared mappings and settings, which is stored across shards. |
| **Document** | A JSON object identified by `_id`, the smallest unit that can be indexed and retrieved. |
| **`_source`** | The original JSON kept with a document, needed for returning results, updates and reindexing. |
| **Primary shard** | The shard copy that accepts a document's writes first; the primary count is set when the index is created. |
| **Replica shard** | A copy of a primary that serves searches and failover; never placed on the primary's node. |
| **Document routing** | Chooses a shard by hashing the routing value, by default `_id`, which is why primary count stays fixed. |
| **Shard sizing** | A common guide is 10 to 50 GB per shard; many tiny shards waste heap and coordination *(verify)*. |
| **Split and shrink APIs** (split and shrink Application Programming Interfaces) | Change primary count by multiples or factors of it, with less cost than a full reindex. |
| **Alias** | A stable name for one or more indices, which allows reindexing without changing client code. |
| **Data stream** | One name for append-only time series data, backed by hidden indices and rolled over automatically. |
| **Index template** | Applies settings and mappings to new indices whose names match a pattern, and can use reusable component templates. |
| **Reindex** | Copies documents into a new index, often needed for mapping or analyser changes that existing data cannot take. |

```text
shard = hash(_routing) % number_of_primary_shards
```

## Write path and storage

| Term | What it means |
|---|---|
| **Translog** (transaction log) | An on-disk record of operations not yet in a Lucene commit, which is replayed during recovery. |
| **`index.translog.durability`** | `request`, the default, syncs the translog before acknowledging; `async` is faster but can lose recent writes. |
| **Refresh** | Writes buffered documents to a new searchable segment, by default every second on actively searched indices. |
| **`refresh_interval`** | How often refresh runs; raising it speeds heavy indexing, but documents become searchable later. |
| **Segment** | An immutable Lucene file set inside a shard; every search visits each segment, so too many slow search. |
| **Flush** | Performs a Lucene commit, persisting segments to disk and letting older translog entries be removed. |
| **Merge** | Combines small segments into larger ones in the background, removing deleted documents at an I/O cost. |
| **Force merge** | Manually merges segments; use it only on indices no longer being written to. |
| **Delete marker** | Deletes only mark a document, and updates mark the old version and add a new one, until merging. |
| **Sequence number** | Per-shard operation order used in recovery and optimistic concurrency, through `if_seq_no` and `if_primary_term`. |
| **Bulk API** (Bulk Application Programming Interface) | Indexes many documents in a single request, far faster than single calls; check per-item errors. |
| **Doc values** | Columnar on-disk data used for sorting and aggregations, instead of loading field data into heap. |

```text
client -> coordinating node -> primary shard -> replica shards -> acknowledge
write  -> buffer + translog -> refresh -> segment -> flush (commit) -> merge
```

| Operation | Makes data searchable | Makes data durable | Cost |
|---|---|---|---|
| Translog sync | No | Yes | Low |
| Refresh | Yes | No | Medium |
| Flush | Not its purpose | Yes, as a Lucene commit | Higher |
| Merge | Not its purpose | Not its purpose | High I/O |

## Mapping and analysis

| Term | What it means |
|---|---|
| **Mapping** | Defines each field's type and indexing; an existing field's type generally cannot change without reindexing. |
| **`dynamic: strict`** | Rejects documents with unknown fields, preventing bad types and mapping explosion. |
| **Multi-field** | Indexes one value several ways, such as `title` as `text` and `title.keyword` as `keyword`. |
| **`object` type** | Flattens inner fields, so values from different array items can falsely match together. |
| **`nested` type** | Indexes array items as hidden separate documents, keeping pairs correct, at extra query and indexing cost. |
| **`flattened` type** | Maps a whole JSON object as a single field, which controls field count but limits querying. |
| **`dense_vector` type** | Stores embeddings for kNN (k-Nearest Neighbours) search, using an HNSW (Hierarchical Navigable Small World) index. |
| **Runtime field** | Computed at query time without reindexing, which is flexible but slower than indexed fields. |
| **Analyser** | Character filters, then one tokeniser, then token filters, producing the terms stored in the inverted index. |
| **`standard` analyser** | The default: splits on word boundaries and lowercases, with no stemming or stop words. |
| **Search analyser** | The analyser used on query text; it can differ from the index analyser, for example with edge n-grams. |
| **Normaliser** | Applies character and token filters to `keyword` fields without tokenising, enabling case-insensitive exact match. |
| **Edge n-gram** | Indexes word prefixes such as `el`, `ela` and `elas`, supporting search-as-you-type at a storage cost. |
| **`_analyze` API** (`_analyze` Application Programming Interface) | Shows the tokens an analyser produces, which is the quickest way to debug missed matches. |
| **Inverted index** | A sorted term dictionary pointing to postings lists of matching documents, which makes term lookups fast. |
| **Postings list** | For each term, the matching document IDs and optionally frequencies and positions, used by scoring and phrases. |

| Field type | Analysed | Use for | Avoid for |
|---|---|---|---|
| `text` | Yes | Full-text search | Sorting and aggregations |
| `keyword` | No, or normalised | Filters, sorting, aggregations | Full-text search |

## Relevance and search execution

| Term | What it means |
|---|---|
| **BM25** (Best Match 25) | The default scorer, combining rare terms, term frequency with saturation, and shorter fields scoring higher. |
| **`k1`** | Controls BM25's term frequency saturation, defaulting to 1.2; lower values make repeats matter less. |
| **`b`** | Controls BM25's field length normalisation, defaulting to 0.75; zero turns it off. |
| **Shard-level IDF** (shard-level Inverse Document Frequency) | Term rarity is calculated per shard by default, so small indices can give slightly inconsistent scores. |
| **Query then fetch** | Shards return top IDs and scores, then the coordinator fetches full documents from the winning shards. |
| **`function_score`** | Adjusts scores using factors such as recency or popularity, on top of text relevance. |
| **Node query cache** | Caches frequently used filter results per segment, one reason to put yes-or-no conditions in `filter`. |
| **Shard request cache** | Caches shard-level results, most often `size: 0` aggregations, until a refresh changes the data. |

## Query DSL

The DSL (Domain-Specific Language) is JSON sent to `_search`. This example scores on text, filters without scoring, and adds a facet.

```text
GET /products/_search
{
  "query": {
    "bool": {
      "must":   [{ "match": { "title": "wireless headphones" } }],
      "filter": [
        { "term":  { "status": "in_stock" } },
        { "range": { "price": { "lte": 200 } } }
      ]
    }
  },
  "aggs": { "by_brand": { "terms": { "field": "brand.keyword", "size": 5 } } },
  "size": 10
}
```

| Term | What it means |
|---|---|
| **`match_phrase`** | Requires analysed terms in order, with `slop` allowing gaps; it relies on stored positions. |
| **`multi_match`** | Runs a text query over several fields, with per-field boosts and types such as `best_fields`. |
| **`range` query** | Matches values between bounds, commonly dates with expressions such as `now-7d/d`. |
| **`nested` query** | Searches inside `nested` fields, keeping conditions on the same array item. |
| **`terms` aggregation** | Returns top values per bucket; counts can be approximate because each shard returns limited candidates. |
| **`date_histogram` aggregation** | Buckets documents by time interval, which is the basis of most time-series charts. |
| **`cardinality` aggregation** | Estimates distinct counts using HyperLogLog++, trading a small error for much lower memory use. |
| **`from` and `size`** | Offset paging limited to 10,000 hits by default, as deep pages get expensive *(verify)*. |
| **`search_after` with PIT** (`search_after` with Point In Time) | Pages through a consistent view using sort values, the recommended way to go deep. |
| **ES\|QL** (Elasticsearch Query Language) | A piped query language for filtering, transforming and aggregating, separate from the JSON DSL *(verify)*. |

| `bool` clause | Must match | Affects score | Cacheable |
|---|---|---|---|
| `must` | Yes | Yes | No |
| `should` | Optional, unless no `must` or `filter` | Yes | No |
| `filter` | Yes | No | Yes |
| `must_not` | Must not match | No | Yes |

## Lifecycle, ingest and snapshots

| Term | What it means |
|---|---|
| **ILM** (Index Lifecycle Management) | Policies moving indices through hot, warm, cold, frozen and delete phases, which automates retention and cost. |
| **Rollover** | Writes to a new index when age, size or document limits are reached, keeping shards a healthy size. |
| **Ingest pipeline** | Ordered processors that transform documents inside Elasticsearch before indexing, reducing the need for separate tooling. |
| **Processor** | One ingest step, such as `grok`, `dissect`, `date`, `set`, `rename`, `geoip` or `enrich`. |
| **`on_failure`** | Handles processor errors, such as sending failed documents to another index, instead of rejecting them. |
| **`_simulate` API** (`_simulate` Application Programming Interface) | Tests an ingest pipeline on sample documents without indexing them. |
| **Snapshot** | An incremental backup of indices and cluster state to a repository; the supported way to back up. |
| **Snapshot repository** | Where snapshots live, such as S3, Google Cloud Storage, Azure Blob Storage or a shared filesystem. |
| **SLM** (Snapshot Lifecycle Management) | Takes and expires snapshots on a schedule, which makes backups routine. |
| **Searchable snapshot** | Searches indices held in a snapshot, which lowers cost for cold data; requires a paid licence *(verify)*. |

| ILM phase | Typical action | Hardware |
|---|---|---|
| Hot | Index and query, roll over | Fast SSDs |
| Warm | Read-mostly, shrink, force merge | Cheaper disks |
| Cold | Rarely queried, often searchable snapshot | Cheapest disks or object storage |
| Frozen | Very rare queries, from object storage | Small cache plus object storage |
| Delete | Remove data | None |

## Kibana

| Term | What it means |
|---|---|
| **Kibana** | The web interface for exploring, visualising and managing Elastic Stack data. |
| **Discover** | Explores raw documents with search, filters and time ranges; usually the first stop in log investigations. |
| **Lens** | A drag-and-drop visualisation builder for dashboard panels. |
| **Dev Tools Console** | Runs Elasticsearch REST requests from the browser, which is useful for testing and debugging. |
| **Data view** | Tells Kibana which indices to read and which fields exist; formerly called an index pattern. |
| **KQL** (Kibana Query Language) | A simple filter syntax for the search bar, such as `status:500 and service:checkout`. |
| **Kibana Spaces** | Separates dashboards and saved objects by team or purpose, with access controlled per space. |

## Logstash, Beats and Agent

| Term | What it means |
|---|---|
| **Logstash** | A JVM data processing engine that collects, transforms and forwards events through input, filter and output stages. |
| **Grok** | Parses unstructured text into fields using named patterns; flexible but slower than `dissect`. |
| **Persistent queue** | Stores Logstash in-flight events on disk, so they survive restarts and absorb bursts. |
| **Beats** | Lightweight Go agents that ship one type of data to Elasticsearch or Logstash. |
| **Filebeat** | Tails and ships log files, tracking read positions so restarts do not restart from the beginning. |
| **Elastic Agent** | A single unified agent for logs, metrics and security data, reducing the number of Beats to run. |
| **Fleet** | Centrally manages Elastic Agents and their policies from Kibana. |
| **ECS** (Elastic Common Schema) | Standard field names for events, so data from different sources can be searched and correlated together. |

```text
# A common log pipeline: Kafka buffers bursts and protects Elasticsearch
Filebeat -> Kafka -> Logstash -> Elasticsearch -> Kibana
```

| Choose ingestion | Ingest pipeline | Logstash |
|---|---|---|
| Runs in | Elasticsearch | Separate JVM service |
| Inputs and outputs | Documents arriving at Elasticsearch | Many sources and destinations |
| Buffering | None of its own | Persistent queue |
| Choose for | Simple parsing and enrichment | Complex routing or multiple outputs |

| Licence | Status |
|---|---|
| Elasticsearch and Kibana | AGPLv3 (GNU Affero General Public License version 3) was added in 2024 alongside the Server Side Public License and Elastic License *(verify)* |
| Paid features | Some features, such as searchable snapshots, need a paid tier *(verify)* |
