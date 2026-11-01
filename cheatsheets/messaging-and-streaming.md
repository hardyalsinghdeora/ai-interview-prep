# Messaging and Streaming Cheatsheet

Covers queues and logs, Kafka, RabbitMQ, SQS, Pub/Sub, delivery semantics, change data capture, schema management and Flink stream processing.

> Related: [Kafka and event streaming skill](../skills/kafka-and-event-streaming.md) · [System design](../topics/03-system-design/README.md) · [distributed-systems.md](distributed-systems.md) · [data-engineering.md](data-engineering.md) · [cloud-services.md](cloud-services.md) · [system-design.md](system-design.md) · [RabbitMQ cheatsheet](rabbitmq.md)

## Core concepts

| Term | What it means |
|---|---|
| **Message queue** | Holds messages until a consumer processes and acknowledges them, which is ideal for distributing work. |
| **Log** | An append-only, ordered record that consumers read by position, so messages can be replayed. |
| **Producer** | The application that writes messages to a broker. |
| **Consumer** | The application that reads and processes messages from a broker. |
| **Broker** | The server that receives, stores and delivers messages. |
| **Publish and subscribe** | One message reaches every interested subscriber, which decouples senders from receivers. |
| **Backpressure** | Slowing or limiting producers when consumers cannot keep up, which prevents overload and data loss. |
| **Consumer lag** | How far a consumer is behind the latest message; it is a key health signal. |
| **Fan-out** | Delivering one event to many consumers, enabling independent downstream processing. |

| Queue vs log | Queue | Log |
|---|---|---|
| After consumption | Usually removed | Kept until retention expires |
| Replay | Generally no | Yes, by resetting offset |
| Scaling | Competing consumers | Partitions and consumer groups |
| Ordering | Often weak or per group | Per partition |
| Examples | RabbitMQ, SQS | Kafka, Kinesis |

## Kafka: core

| Term | What it means |
|---|---|
| **Topic** | A named stream of records, split into partitions for scale. |
| **Partition** | An ordered, append-only segment of a topic; it is the unit of parallelism and of ordering. |
| **Offset** | A record's position within a partition, which consumers commit to track progress. |
| **Message key** | Decides the partition, so records with one key stay ordered; skewed keys create hot partitions. |
| **Consumer group** | Consumers sharing work, where each partition goes to one member, so partitions cap useful parallelism. |
| **Rebalance** | Reassigning partitions when group membership changes, which can briefly pause consumption. |
| **Retention** | How long or how much data is kept, independent of whether it has been consumed. |
| **Replication factor** | The number of copies of each partition, commonly three, which determines fault tolerance. |
| **Leader** | The replica handling a partition's writes and, by default, its reads. |
| **Follower** | A replica that copies from the leader and can take over if the leader fails. |
| **ISR** (In-Sync Replicas) | Replicas caught up with the leader; only these are normally eligible to become leader. |
| **`acks`** | Producer setting for required acknowledgements: `0` none, `1` leader, `all` every in-sync replica. |
| **`min.insync.replicas`** | Minimum in-sync replicas needed for `acks=all` writes; a value of two with three replicas is common. |
| **High watermark** | The offset replicated to all in-sync replicas; consumers can only read up to it. |
| **Log compaction** | Keeps at least the latest record per key rather than expiring by time, which is useful for current state. |
| **Tombstone** | A record with a null value that marks a key for deletion during compaction. |
| **Idempotent producer** | Broker removes duplicates caused by producer retries, within a producer session and partition. |
| **Kafka transactions** | Atomic writes across partitions and offsets, which enable exactly-once read-process-write pipelines. |
| **KRaft** (Kafka Raft) | Kafka's built-in metadata consensus; ZooKeeper support was removed in Kafka 4.0 *(verify)*. |
| **Share group** | Queue-style consumption with per-record acknowledgement, production-ready from Kafka 4.2 *(verify)*. |
| **Kafka Connect** | A framework for moving data between Kafka and external systems using reusable connectors. |
| **Kafka Streams** | A Java library for building stream processing into applications, with no separate cluster required. |

## RabbitMQ

| Term | What it means |
|---|---|
| **AMQP** (Advanced Message Queuing Protocol) | The messaging protocol at the heart of RabbitMQ, version 0-9-1 being the classic model. |
| **Exchange** | Receives messages from producers and routes them to queues using rules. |
| **Binding** | The rule linking an exchange to a queue. |
| **Routing key** | A message attribute that exchanges use to choose destination queues. |
| **Direct exchange** | Routes on an exact routing key match. |
| **Topic exchange** | Routes on wildcard patterns, such as `orders.*`. |
| **Fanout exchange** | Copies a message to all bound queues, ignoring the routing key. |
| **Headers exchange** | Routes using message header values rather than a routing key. |
| **Prefetch** | Limits unacknowledged messages per consumer, balancing throughput against fair distribution. |
| **Quorum queue** | A replicated queue based on Raft, recommended for data safety. |
| **Virtual host** | A logical partition of a broker, which separates applications or tenants. |

## SQS and Pub/Sub

| Term | What it means |
|---|---|
| **SQS** (Simple Queue Service) | AWS's managed queue with no brokers to run; maximum payload is 1 MiB *(verify)*. |
| **Standard queue** | SQS default: nearly unlimited throughput, at-least-once delivery and best-effort ordering. |
| **FIFO queue** (First-In, First-Out queue) | SQS option with ordering per message group and deduplication, at lower throughput. |
| **Visibility timeout** | Hides a received message while it is processed; set it longer than the normal processing time. |
| **Long polling** | Waits for messages rather than returning empty immediately, which reduces cost and empty responses. |
| **Message group ID** (message group identifier) | SQS FIFO key that sets ordering scope, with parallelism across groups. |
| **Redrive policy** | Moves a message to a dead letter queue after a set number of failed receives. |
| **Pub/Sub** | Google Cloud's managed messaging service, in which topics deliver messages to subscriptions. |
| **Subscription** | A named stream of a topic's messages for one consumer application. |
| **Acknowledgement deadline** | Time a Pub/Sub subscriber has to acknowledge before the message is redelivered. |
| **Push and pull subscriptions** | Push delivers to an HTTP endpoint, while pull has subscribers request messages at their own pace. |
| **Ordering key** | Pub/Sub attribute that delivers messages with one key in publish order, when enabled. |

## Delivery semantics and failure

| Term | What it means |
|---|---|
| **At-most-once** | Messages may be lost but are never duplicated, which suits disposable telemetry. |
| **At-least-once** | Messages are not lost but may repeat, so consumers must be idempotent; the common default. |
| **Exactly-once** | The effect occurs once; realistic only within a defined boundary or through idempotency, not across arbitrary systems. |
| **Idempotent consumer** | Processing a duplicate has no extra effect, usually through a stored message or business ID. |
| **DLQ** (Dead Letter Queue) | Holds messages that repeatedly fail, so they do not block others; alert on it and plan reprocessing. |
| **Poison message** | A message that always fails processing, which can block progress if retried forever. |
| **Retry with backoff** | Waits progressively longer between attempts, giving failing dependencies room to recover. |
| **Outbox pattern** | Writes the event and business change in one database transaction, then publishes it, avoiding dual-write inconsistency. |
| **Saga** | A sequence of local transactions with compensating actions, used for workflows across services. |

## CDC and schemas

| Term | What it means |
|---|---|
| **CDC** (Change Data Capture) | Streams database inserts, updates and deletes to other systems, avoiding polling and dual writes. |
| **Log-based CDC** (log-based Change Data Capture) | Reads the database's transaction log, which has low impact and captures every change including deletes. |
| **WAL** (Write-Ahead Log) | PostgreSQL's durable change log, a common source for change data capture. |
| **Binlog** (Binary Log) | MySQL's change log, used for replication and change data capture. |
| **Debezium** | An open-source change data capture platform, commonly running on Kafka Connect. |
| **Schema registry** | A central store of versioned message schemas that enforces compatibility between producers and consumers. |
| **Backward compatibility** | Consumers using a new schema can read old data, so upgrade consumers first. |
| **Forward compatibility** | Consumers using an old schema can read new data, so upgrade producers first. |
| **Full compatibility** | Both backward and forward compatibility hold, offering the most deployment freedom. |
| **Avro** | A compact binary format with schemas, popular with Kafka. |
| **Protobuf** (Protocol Buffers) | A compact binary format using numbered fields, well suited to schema evolution. |

## Flink and stream processing

| Term | What it means |
|---|---|
| **Stream processing** | Computing continuously on unbounded events as they arrive, rather than in periodic batches. |
| **Event time** | When an event actually happened, which gives correct results with out-of-order data. |
| **Processing time** | When the system handles an event; it is simple but changes across replays. |
| **Watermark** | A signal that events up to a time are probably complete, which tells windows when to close. |
| **Late data** | Events arriving after the watermark passed; these can be dropped, redirected or used to update results. |
| **Allowed lateness** | How long a window stays updatable after the watermark passes it. |
| **Tumbling window** | Fixed, non-overlapping time buckets, such as every five minutes. |
| **Sliding window** | Fixed-size overlapping windows, such as ten minutes advancing every minute. |
| **Session window** | Groups events separated by less than an inactivity gap, so window size varies. |
| **Keyed state** | Data stored per key within an operator, which allows aggregations across events. |
| **Checkpoint** | An automatic consistent snapshot of state, used for failure recovery and exactly-once state. |
| **Savepoint** | A manually triggered state snapshot, used for upgrades and rescaling. |
| **JobManager** | Flink's coordinator, which schedules tasks and manages checkpoints. |
| **TaskManager** | A Flink worker process that executes tasks in its slots. |
