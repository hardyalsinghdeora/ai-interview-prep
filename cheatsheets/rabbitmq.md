# RabbitMQ Cheatsheet

Covers the AMQP model, exchanges and routing, queue types, acknowledgements and confirms, dead lettering and TTL, streams, federation and shovel, with a comparison against Kafka and SQS.

> Related: [messaging-and-streaming.md](messaging-and-streaming.md) · [RabbitMQ, SQS and task queues skill](../skills/rabbitmq-sqs-and-task-queues.md) · [Kafka and event streaming skill](../skills/kafka-and-event-streaming.md) · [aws.md](aws.md) · [distributed-systems.md](distributed-systems.md) · [System design](../topics/03-system-design/README.md) · [RabbitMQ & other brokers skill](../skills/rabbitmq-and-other-brokers.md)

## Core model

| Term | What it means |
|---|---|
| **RabbitMQ** | An open-source message broker, written in Erlang, known for flexible routing; licensed under MPL 2.0 *(verify)*. |
| **AMQP 0-9-1** (Advanced Message Queuing Protocol version 0-9-1) | RabbitMQ's classic wire protocol, which defines exchanges, queues and bindings and lets applications declare them. |
| **AMQP 1.0** (Advanced Message Queuing Protocol version 1.0) | A different, peer-to-peer standard, supported natively by RabbitMQ from version 4.0 *(verify)*. |
| **Broker** | The server that accepts, routes, stores and delivers messages. |
| **Node** | One running RabbitMQ server, which can stand alone or join a cluster. |
| **Virtual host** (vhost) | A logical broker namespace with its own exchanges, queues and permissions, separating applications or tenants. |
| **Connection** | A long-lived TCP connection to the broker; costly to open, so applications should reuse them. |
| **Channel** | A lightweight virtual connection inside a TCP connection; use one per thread rather than sharing it. |
| **Producer** | The application that publishes messages to an exchange, never directly to a queue. |
| **Consumer** | The application that receives messages from a queue and acknowledges them. |
| **Message properties** | Metadata such as `delivery_mode`, `content_type`, `correlation_id`, `reply_to` and `expiration`, carried alongside the payload. |
| **Persistent message** | A message with `delivery_mode=2`, written to disk, though it needs a durable queue to survive restarts. |

```text
producer -> exchange -> (binding match on routing key) -> queue -> consumer
```

## Exchanges and routing

| Term | What it means |
|---|---|
| **Exchange** | Receives published messages and routes them to queues or other exchanges, but does not store them. |
| **Direct exchange** | Routes to queues whose binding key exactly matches the routing key, which suits task distribution by type. |
| **Topic exchange** | Routes by matching dot-separated routing keys against patterns using `*` and `#`, which suits filtered subscriptions. |
| **Fanout exchange** | Copies each message to every bound queue, ignoring the routing key, which suits broadcasts. |
| **Headers exchange** | Routes on header values using `x-match` set to `all` or `any`; useful when one routing key is too limiting, but less common. |
| **Default exchange** | A built-in nameless direct exchange that binds every queue using the queue's name as the key. |
| **Binding** | A rule linking an exchange to a queue or another exchange, usually with a binding key. |
| **Routing key** | A message attribute set by the publisher, which exchanges compare with binding keys. |
| **Alternate exchange** | Receives messages that an exchange cannot route, so unmatched messages are kept rather than dropped. |
| **`mandatory` flag** | Asks the broker to return an unroutable message to the publisher instead of silently discarding it. |

| Exchange type | Routes by | Example |
|---|---|---|
| Direct | Exact key | `billing` binds to key `billing` |
| Topic | Pattern | `orders.*.created` |
| Fanout | Nothing, copies to all | Cache invalidation broadcast |
| Headers | Header values | `format=pdf` and `type=report` |

```text
# Topic wildcards: * is exactly one word, # is zero or more words
routing key: orders.eu.created

orders.*.created   match
orders.#           match
*.created          no match (key has three words)
#.created          match
```

## Queues

| Term | What it means |
|---|---|
| **Queue** | An ordered buffer storing messages until consumers take them; the basic storage object. |
| **Classic queue** | The traditional, non-replicated queue type, which is fine for transient or lower-value data. |
| **Quorum queue** | A durable queue replicated with Raft, tolerating a minority of node failures; the default choice for data safety. |
| **Stream** | A replicated, append-only log whose messages stay after being read, allowing replay and many consumers. |
| **Durable queue** | A queue whose definition survives a broker restart; messages also need persistence to survive. |
| **Exclusive queue** | Usable only by its declaring connection and deleted when that connection closes, as with temporary reply queues. |
| **Auto-delete queue** | Deleted when its last consumer unsubscribes, which suits temporary subscriptions. |
| **Queue leader** | The quorum queue replica handling operations, with followers ready to take over through Raft election. |
| **Classic mirrored queue** | The old replication feature, removed in RabbitMQ 4.0; migrate to quorum queues *(verify)*. |
| **Queue length limit** | `x-max-length` or `x-max-length-bytes` bounds a queue, and overflow behaviour drops old messages or rejects new ones. |
| **Priority queue** | Delivers higher-priority messages first, with details differing between classic and quorum queues *(verify)*. |
| **Single active consumer** | Lets only one consumer read a queue at a time, with failover, preserving processing order. |

| Choose queue type | Classic | Quorum | Stream |
|---|---|---|---|
| Replicated | No | Yes, with Raft | Yes |
| After consumption | Removed | Removed | Kept until retention |
| Replay | No | No | Yes, by offset |
| Choose for | Temporary, non-critical data | Durable work queues | Fan-out, replay, large backlogs |

## Consumer and publisher reliability

| Term | What it means |
|---|---|
| **Acknowledgement** | A consumer's signal that a message was handled, so the broker can remove it. |
| **Automatic acknowledgement** | The broker treats a message as handled on delivery, which is fast but loses messages on crashes. |
| **Manual acknowledgement** | The consumer acknowledges after processing, giving at-least-once delivery, so handlers should be idempotent. |
| **`basic.nack`** | Negatively acknowledges one or many messages, with a choice of requeueing or dead lettering. |
| **`basic.reject`** | Negatively acknowledges a single message, the older protocol equivalent of `basic.nack`. |
| **Redelivered flag** | Marks a message that may have been delivered before, hinting that a duplicate is possible. |
| **Prefetch** | Set with `basic.qos`, this limits unacknowledged messages per consumer, so slower consumers are not overloaded. |
| **Consumer timeout** | Closes the channel if a delivery stays unacknowledged too long, by default 30 minutes *(verify)*. |
| **`basic.consume` vs `basic.get`** | Consume subscribes and receives pushed messages; get polls one at a time and is inefficient. |
| **Publisher confirm** | The broker asynchronously acknowledges a published message once it has taken responsibility for it, giving at-least-once publishing. |
| **Transaction** | AMQP `tx` commands make publishes atomic, but they are slow, so publisher confirms are usually better. |
| **Flow control** | Slows publishing connections that send faster than the broker can process, as a form of backpressure. |
| **Memory and disk alarms** | Block publishers when memory or free disk crosses a threshold, protecting the broker from running out. |

| Prefetch value | Effect |
|---|---|
| `1` | Fairest distribution, lowest throughput; suits slow, uneven tasks |
| Tens to low hundreds | Common balance of throughput and fairness *(verify)* |
| Unlimited | Risks overwhelming consumers and uneven distribution |

```text
Safe at-least-once path:
publisher confirms + durable queue + persistent messages + manual acks + idempotent consumer
```

## Dead lettering, TTL and retries

| Term | What it means |
|---|---|
| **DLX** (Dead Letter Exchange) | Where a queue republishes dead messages, which keeps failures for inspection or retry. |
| **Dead letter causes** | Rejected without requeue, TTL expired, queue length exceeded or a quorum queue's delivery limit reached. |
| **`x-dead-letter-routing-key`** | Replaces the routing key on dead-lettered messages; otherwise the original key is used. |
| **`x-death` header** | Added on dead lettering, recording the source queue, reason and count, used to limit retries. |
| **Delivery limit** | A quorum queue's cap on redeliveries, stopping poison messages from looping; the default changed in 4.0 *(verify)*. |
| **TTL** (Time To Live) | How long a message may wait in a queue before it expires, either dropped or dead-lettered. |
| **Message TTL** (message Time To Live) | Set per queue with `x-message-ttl` or per message with `expiration`; the lower value applies. |
| **Queue TTL** (queue Time To Live) | `x-expires` deletes an unused queue after a set period, which cleans up abandoned queues. |
| **Retry with TTL and DLX** (retry with Time To Live and Dead Letter Exchange) | Failed messages wait in a delay queue with no consumers, then dead-letter back to the work queue. |
| **Delayed message exchange** | A community plugin that holds messages until a set delay, with known limitations *(verify)*. |

## Streams

| Term | What it means |
|---|---|
| **Stream offset** | A consumer's position in a stream, which can start at `first`, `last`, `next`, an offset or a timestamp. |
| **Stream retention** | Removes old stream data by age or size, rather than when messages are consumed. |
| **Stream protocol** | A dedicated binary protocol with much higher throughput than reading streams over AMQP *(verify)*. |
| **Offset tracking** | Stores a named consumer's position on the broker, so it can resume after restarting. |
| **Super stream** | A partitioned stream made of several streams, which adds scale and per-partition ordering *(verify)*. |

## Clustering and links between brokers

| Term | What it means |
|---|---|
| **Cluster** | Nodes sharing users, vhosts, exchanges and queue definitions, intended for one low-latency network. |
| **Network partition** | Cluster nodes lose contact with each other; `pause_minority` is the usual safe handling strategy *(verify)*. |
| **Khepri** | A Raft-based metadata store replacing the older Mnesia, improving behaviour during partitions *(verify)*. |
| **Federation** | Moves messages from upstream exchanges or queues to a downstream broker over AMQP, across WANs and organisations. |
| **Shovel** | Reliably moves messages from a source to a destination, used for migrations and bridging brokers. |
| **Policy** | Applies optional queue and exchange settings by name pattern at runtime, avoiding hard-coded arguments in applications. |
| **Management plugin** | Provides the web UI and HTTP API for monitoring and administration, on port 15672 by default. |
| **Prometheus plugin** | Exposes metrics for scraping; watch queue depth, unacknowledged counts, publish rates and alarms. |
| **Amazon MQ** | AWS's managed broker service, which offers RabbitMQ and ActiveMQ engines *(verify)*. |

| Connect brokers | Cluster | Federation | Shovel |
|---|---|---|---|
| Topology | One logical broker | Loosely linked brokers | Point-to-point links |
| Network | Reliable, low latency | WAN tolerated | WAN tolerated |
| Direction | Shared metadata | Upstream to downstream | Source to destination |
| Choose for | Availability in one region | Distributing across regions or organisations | Migration or moving specific messages |

## RabbitMQ vs Kafka vs SQS

| Comparison | RabbitMQ | Kafka | SQS |
|---|---|---|---|
| Model | Smart broker, queues and exchanges | Partitioned, replicated log | Managed queue |
| Delivery to consumers | Push, with prefetch | Consumers pull | Consumers poll |
| After consumption | Removed, except streams | Kept until retention | Deleted by the consumer |
| Replay | Streams only | Yes | No |
| Routing | Rich: direct, topic, fanout, headers | By topic and key | None; add SNS or EventBridge |
| Ordering | Per queue, until redelivery or competing consumers | Per partition | Per message group on FIFO |
| Scaling | Competing consumers, more queues | More partitions and brokers | Automatic |
| Failed messages | Dead letter exchange, delivery limits | Application-level retry topics | Redrive policy to a dead letter queue |
| Operations | Run a cluster, or use a managed service | Run a cluster, or use a managed service | None |
| Choose for | Task queues and complex routing | Event streaming and replay at high throughput | Simple AWS work queues with no brokers |
