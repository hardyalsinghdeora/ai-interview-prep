# Distributed Systems Cheatsheet

Core terms for building reliable systems across many machines, covering consistency, consensus, time, messaging guarantees and failure handling.

> Related: [system-design.md](system-design.md) · [databases-and-sql.md](databases-and-sql.md) · [messaging-and-streaming.md](messaging-and-streaming.md) · [observability-and-reliability.md](observability-and-reliability.md) · [System design topic](../topics/03-system-design/README.md) · [Large-scale distributed systems skill](../skills/large-scale-distributed-systems.md) · [Kafka and event streaming skill](../skills/kafka-and-event-streaming.md)

## CAP and PACELC

| Term | What it means |
|---|---|
| **CAP theorem** (Consistency, Availability, Partition tolerance) | During a network partition, a system must give up either linearisable consistency or availability. Frames the failure-mode trade-off, not everyday design. |
| **PACELC** (Partition, Availability, Consistency, Else, Latency, Consistency) | Extends CAP: under Partition choose Availability or Consistency; Else choose Latency or Consistency. Captures the everyday cost of coordination. |
| **Network partition** | Nodes are running but cannot reach one another. Inevitable at scale, so partition tolerance is not really optional. |
| **CP system** (Consistency and Partition tolerance) | Refuses some requests during a partition rather than return stale or conflicting data. Suits locks, metadata and financial records. |
| **AP system** (Availability and Partition tolerance) | Keeps answering during a partition and reconciles later. Suits carts, feeds and data where staleness is acceptable. |

## Consistency models

| Term | What it means |
|---|---|
| **Linearisability** | Each operation appears to take effect at one instant, respecting real-time order. Behaves like a single copy, at a coordination cost. |
| **Sequential consistency** | All nodes see one operation order that respects each client's order, but not real time. Weaker and cheaper than linearisability. |
| **Causal consistency** | Everyone sees cause before effect; unrelated writes may appear in any order. A strong model that stays available during partitions. |
| **Eventual consistency** | Without new writes, replicas converge eventually. Highly available and fast, but readers may see stale values meanwhile. |
| **Strong eventual consistency** | Replicas that received the same updates hold the same state, with no coordination. The guarantee CRDTs provide. |
| **Read-your-writes** | A client always sees its own earlier writes. Avoids the confusion of a saved change seeming to disappear. |
| **Monotonic reads** | A client never sees data go backwards in time. Often achieved by pinning a client to one replica. |
| **Monotonic writes** | A client's writes apply in the order issued. Prevents older updates overwriting newer ones from the same client. |

| Model | Guarantee | Cost |
|---|---|---|
| Linearisable | Single up-to-date copy, real-time order | Highest latency, unavailable in a minority partition |
| Sequential | One global order per client order | High coordination |
| Causal | Cause before effect | Dependency tracking |
| Eventual | Convergence only | Stale reads, conflict handling |

## Consensus

| Term | What it means |
|---|---|
| **Consensus** | Getting nodes to agree on a value despite failures. Underpins leader election, configuration and replicated logs. |
| **State machine replication** | Replicas apply the same ordered log of commands, so they reach the same state. The main practical use of consensus. |
| **FLP impossibility** (Fischer, Lynch, Paterson) | In a fully asynchronous network, no deterministic algorithm guarantees consensus with even one crash. Real systems rely on timeouts to progress. |
| **Paxos** | Foundational consensus algorithm using proposers, acceptors and majority quorums. Proven and widely adapted, but hard to understand and implement. |
| **Crash fault** | A node stops working but does not lie. Paxos and Raft tolerate these, needing 2f+1 nodes for f failures. |
| **Byzantine fault** | A node behaves arbitrarily or maliciously. Far harder to handle, typically needing 3f+1 nodes for f failures. |
| **BFT** (Byzantine Fault Tolerance) | Reaching agreement despite Byzantine nodes. Used in blockchains and high-assurance systems, rarely inside a trusted data centre. |

## Raft

| Term | What it means |
|---|---|
| **Raft** | Understandable consensus algorithm with one leader replicating a log. Used by etcd and many other modern systems. |
| **Term** (Raft) | Numbered period of logical time beginning with an election. Stale leaders are detected by their lower term. |
| **Election timeout** | Randomised wait after which a follower becomes a candidate. Randomness reduces split votes, which delay electing a leader. |
| **Heartbeat** | Empty message from the leader that maintains authority. Missing heartbeats trigger a new election. |
| **Log replication** (Raft) | Leader appends entries, then sends them to followers and retries until they match. Keeps every replica's history identical. |
| **Commit index** | Highest log entry known safely stored on a majority. Only committed entries are applied to the state machine. |
| **Election restriction** | Nodes vote only for candidates whose log is at least as up to date as theirs. Stops a new leader losing committed data. |
| **Log compaction** | Replaces old log entries with a state snapshot. Limits disk use and speeds up catching up new nodes. |

```text
Quorum = floor(N / 2) + 1
3 nodes: quorum 2, tolerates 1 failure
4 nodes: quorum 3, tolerates 1 failure (no gain over 3)
5 nodes: quorum 3, tolerates 2 failures
```

## Quorums

| Term | What it means |
|---|---|
| **Quorum** | Minimum number of nodes that must respond for an operation to count. Overlapping quorums let reads find the latest write. |
| **N, W and R** (Replicas, Write quorum, Read quorum) | Configuration values for leaderless stores. `R + W > N` means read and write sets overlap. |
| **Sloppy quorum** | Writes go to any reachable nodes when home replicas are down. Improves availability, but quorum overlap is no longer guaranteed. |
| **Hinted handoff** | A stand-in node stores writes, then forwards them when the proper replica returns. Pairs with sloppy quorums to restore placement. |
| **Read repair** | A read that finds outdated replicas updates them. Heals frequently read data without any separate job. |
| **Anti-entropy** | Background comparison and repair between replicas. Fixes data that reads rarely touch. |
| **Merkle tree** | Tree of hashes summarising a data range. Finds differences between replicas while transferring very little data. |

## Leader election and coordination

| Term | What it means |
|---|---|
| **Leader election** | Choosing one node to coordinate work. Simplifies ordering and writes, but needs safe handling when leaders change. |
| **Lease** | Time-limited ownership that must be renewed. Lets a dead holder lose its role automatically, but depends on bounded clock drift. |
| **Fencing token** | Increasing number issued with each lock or lease. Storage rejects older numbers, blocking paused former leaders from corrupting data. |
| **Bully algorithm** | The highest-numbered reachable node claims leadership. A simple teaching example that is not safe under network partitions. |
| **Coordination service** | Consensus-backed store such as etcd or ZooKeeper. Provides locks, leases, watches and configuration, so applications avoid writing consensus themselves. |
| **Failure detector** | Infers node failure from missed heartbeats. It can only suspect, because slow and dead nodes look alike over a network. |
| **Gossip protocol** | Nodes periodically share state with random peers. Spreads membership and metadata scalably, with eventual convergence. |

## Clocks and ordering

| Term | What it means |
|---|---|
| **Wall clock** | Time-of-day clock, which can jump when corrected. Unsafe for measuring durations or ordering events across machines. |
| **Monotonic clock** | Timer that only moves forward. Use it for durations and timeouts within a single machine. |
| **NTP** (Network Time Protocol) | Synchronises machine clocks over the network. Reduces skew but gives no strict bound, so do not rely on it for correctness. |
| **Clock skew** | Difference between clocks on two machines. Can make later events appear earlier and lose data under last-write-wins. |
| **Happens-before** | Partial order where A precedes B if A could have influenced B. The foundation of logical clocks. |
| **Lamport timestamp** | Counter updated on each event and message. If A happened before B, A's value is lower; the reverse does not follow. |
| **Vector clock** | One counter per node. Detects whether two events are causally ordered or concurrent, at the cost of metadata growth. |
| **HLC** (Hybrid Logical Clock) | Combines physical time with a logical counter. Timestamps stay near real time while still preserving causality. |
| **TrueTime** | Google Spanner's clock interface returning a time interval with bounded uncertainty. Commit-wait uses it for externally consistent transactions. |

## Idempotency and delivery semantics

| Term | What it means |
|---|---|
| **Idempotency** | Applying an operation repeatedly has the same effect as once. Makes retries safe on unreliable networks. |
| **Idempotency key** | Client-generated ID stored with the result of a request. Repeats return the original result, preventing double charges. |
| **At-most-once delivery** | A message is sent with no retries and may be lost, but never repeated. Suits disposable data such as some metrics. |
| **At-least-once delivery** | Retries until acknowledged, so duplicates can occur. The common default; consumers must therefore be idempotent. |
| **Exactly-once delivery** | Each message delivered once. Not achievable in general over unreliable networks, so systems approximate it. |
| **Effectively-once processing** | Duplicates may arrive, but the outcome reflects one processing. Built from at-least-once plus deduplication or atomic commits. |
| **Deduplication** | Dropping repeated messages using a unique ID store. Needs a retention window at least as long as possible redelivery. |
| **DLQ** (Dead Letter Queue) | Holding area for messages that repeatedly fail. Stops poison messages blocking processing, and allows inspection and replay. |

| Semantic | Loss possible | Duplicates possible | Needs |
|---|---|---|---|
| At-most-once | Yes | No | Nothing extra |
| At-least-once | No, if acknowledged properly | Yes | Retries, idempotent consumer |
| Effectively-once | No, if acknowledged properly | Yes, without visible effect | Deduplication or transactional processing |

## Saga, outbox and distributed transactions

| Term | What it means |
|---|---|
| **2PC** (Two-Phase Commit) | Coordinator asks participants to prepare, then to commit. Gives atomicity across systems, but blocks if the coordinator fails. |
| **Dual write** | Updating two systems with no shared transaction, such as a database and a broker. A crash between them leaves inconsistent data. |
| **Saga** | Sequence of local transactions, each with a compensating step. Provides cross-service workflows without long-held distributed locks. |
| **Compensating transaction** | Business action that semantically undoes an earlier step, such as a refund. Not a true rollback, because others may have seen the effect. |
| **Choreography** | Saga steps react to each other's events. Loosely coupled with no central component, but the overall flow is harder to follow. |
| **Orchestration** | Central coordinator tells each service what to do next. Clearer visibility and error handling, but adds a component to run. |
| **Transactional outbox** | Writes the event to an outbox table in the same transaction as the data; a relay then publishes it. Solves dual writes. |
| **Inbox pattern** | Consumer records processed message IDs in its own transaction. Pairs with the outbox for idempotent, effectively-once handling. |

| 2PC vs Saga | 2PC | Saga |
|---|---|---|
| Isolation | Strong, locks held | None between steps |
| Failure handling | Roll back through coordinator | Compensating actions |
| Availability | Blocks on coordinator failure | Local commits continue |
| Duration | Short operations | Long-running workflows |
| Intermediate states | Hidden | Visible to other transactions |

## CRDTs

| Term | What it means |
|---|---|
| **CRDT** (Conflict-free Replicated Data Type) | Data structure whose replicas can update independently and always merge safely. Suits collaboration, offline use and multi-region writes. |
| **State-based CRDT** (Conflict-free Replicated Data Type) | Replicas exchange full state and merge it using a commutative, associative and idempotent function. Simple, but state can be large. |
| **Operation-based CRDT** (Conflict-free Replicated Data Type) | Replicas exchange operations rather than state. Less data to send, but delivery must be reliable and causally ordered. |
| **G-Counter** (Grow-only Counter) | One increment-only count per node; the value is their sum and merging takes each maximum. Suits likes or view counts. |
| **PN-Counter** (Positive-Negative Counter) | Two G-Counters, one for increments and one for decrements. Provides a counter that can also go down. |
| **LWW-Register** (Last-Write-Wins Register) | Keeps the value with the latest timestamp. Simple, but concurrent writes are silently dropped and clock skew matters. |
| **OR-Set** (Observed-Remove Set) | Tags each addition uniquely, and a removal only cancels tags it has seen. Concurrent re-adds survive. |

## Backpressure and resilience

| Term | What it means |
|---|---|
| **Backpressure** | Signalling producers to slow down when consumers cannot keep up. Prevents unbounded queues and cascading overload. |
| **Bounded queue** | Queue with a fixed limit, which blocks or rejects when full. Makes overload visible rather than exhausting memory. |
| **Load shedding** | Deliberately rejecting some work when overloaded, often lower-priority requests. Keeps the remaining traffic within acceptable latency. |
| **Timeout** | Maximum wait for a response. Without timeouts, slow dependencies tie up threads and connections, spreading failure. |
| **Deadline propagation** | Passes the remaining time budget to downstream calls. Stops services doing work for callers that have already given up. |
| **Exponential backoff** | Increases the wait between retries, often doubling each time. Gives a struggling dependency time to recover. |
| **Jitter** | Random variation added to retry delays. Stops many clients retrying at once in synchronised waves. |
| **Retry storm** | Retries multiply traffic against a failing service, sometimes at several layers. Limit with retry budgets and retries at one layer. |
| **Circuit breaker** | Stops calling a failing dependency and fails fast for a period. Protects both caller and dependency, then allows recovery. |
| **Half-open state** | Circuit breaker lets a few trial requests through after cooling off. Success closes the circuit; failure opens it again. |
| **Bulkhead** | Isolates resources, such as a thread pool per dependency. One failing dependency then cannot consume all capacity. |
| **Hedged request** | Sends a second request if the first is slow, and uses whichever returns first. Cuts tail latency for safe, idempotent reads. |
| **Thundering herd** | Many waiting clients act at once after an event, such as a cache expiry or recovery. Can immediately overload the system again. |

```text
Circuit breaker: Closed --(failures over threshold)--> Open --(cool-off elapsed)--> Half-open
                 Half-open --(trial succeeds)--> Closed | Half-open --(trial fails)--> Open

Retry delay:     random(0, min(max_delay, base * 2^attempt))   // exponential backoff, full jitter
```

## Split brain and partial failure

| Term | What it means |
|---|---|
| **Split brain** | Two sides of a partition both believe they are leader and accept writes. Causes divergence and data loss that is hard to repair. |
| **Fencing** | Cutting off a suspected dead node from shared resources. Ensures a former leader cannot keep writing after failover. |
| **STONITH** (Shoot The Other Node In The Head) | Forcibly powers off a suspect node before failover. A blunt but reliable form of fencing in high-availability clusters. |
| **Majority rule** | Only the side holding a quorum may continue. Prevents split brain, at the cost of the minority side becoming unavailable. |
| **Witness node** | Lightweight voter that helps break ties without a full data copy. Lets two data sites avoid split brain. |
| **Grey failure** | A component is partly broken yet passes health checks, such as being slow or dropping some packets. Difficult to detect and diagnose. |
| **Cascading failure** | One overloaded or failed component pushes load onto others until they fail too. Contain with limits, shedding and isolation. |
