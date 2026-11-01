# RabbitMQ & other brokers

RabbitMQ looks simple until a queue stops draining or a network partition splits the cluster, so interviewers who have run it in production go past "what is an exchange" into confirms, prefetch, quorum queues and recovery, then ask when a different broker would have been the better call. Senior backend, platform and Solutions Architect roles ask most, often as a design choice between RabbitMQ, Kafka, ActiveMQ, NATS and Pulsar.

> Related: [RabbitMQ, SQS & task queues](rabbitmq-sqs-and-task-queues.md) · [Kafka & event streaming](kafka-and-event-streaming.md) · [System design](../topics/03-system-design/README.md) · [Cloud, DevOps & Integration](../topics/08-cloud-devops-integration/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] How does the default exchange let you publish to a queue by name without declaring a binding, and why should a real topology not lean on it?
- [ ] Given the routing key `orders.eu.created`, which of these topic bindings match: `orders.*.created`, `orders.#`, `*.created`, `#.created`? Explain why.
- [ ] When would a headers exchange suit you better than a topic exchange, what do `x-match: all` and `x-match: any` change, and why is it rarely chosen?
- [ ] A queue has two bindings that both match one message. How many copies does it receive, and what changes if the bindings point at two different queues?
- [ ] What happens to a message that matches no binding? Compare the `mandatory` flag with an alternate exchange.
- [ ] Durable, exclusive and auto-delete queues: what does each property mean, and what does a durable queue not guarantee on its own?
- [ ] Connections vs. channels: why multiplex channels over one connection, why not share a channel across threads, and what does opening a connection per publish do to a broker?
- [ ] Automatic vs. manual acknowledgements: what guarantee does each give, what is the scope of a delivery tag, and what does `multiple=true` do?
- [ ] How does consuming from a RabbitMQ stream differ from consuming from a queue, and what happens to a message after it has been read? *(verify)*

## B. Working depth
- [ ] How would you choose a prefetch value for a 5 ms handler compared with a 30-second handler, and which metrics tell you it is wrong?
- [ ] Per-consumer vs. per-channel prefetch: what does the `global` flag do in RabbitMQ, how does that differ from the AMQP 0-9-1 specification, and what do quorum queues support? *(verify)*
- [ ] Publisher confirms: compare waiting per message, batching and asynchronous confirm handling for throughput and for knowing which messages to republish.
- [ ] A publisher uses confirms but not the `mandatory` flag. A message is confirmed yet never reaches a queue. How is that possible, and how do you detect it?
- [ ] When exactly does the broker confirm a persistent message on a durable classic queue compared with a quorum queue, and why does confirm latency climb under load? *(verify)*
- [ ] What happens when a consumer holds an unacknowledged message past `consumer_timeout`, and how would you design for work that runs longer than that? *(verify)*
- [ ] How does Raft replication in a quorum queue decide a message is safe, why use an odd number of replicas, and what happens when a majority is lost?
- [ ] Quorum queue `delivery-limit`: what does it count, and what happens to a message that reaches the limit with and without a dead-letter exchange configured? *(verify)*
- [ ] Which events cause a message to be dead-lettered, how do `x-dead-letter-exchange` and `x-dead-letter-routing-key` shape where it goes, and how would you read `x-death` to drive a retry count?
- [ ] Why can a long-TTL message at the head of a queue hold back a short-TTL message behind it, and how do tiered delay queues get around that?
- [ ] The delayed message exchange plugin: where does it hold messages before delivery, and what limits it for large volumes or high availability? *(verify)*
- [ ] Streams: how do offset specifications, server-side offset tracking and retention work, and when would you use the native stream protocol rather than AMQP? *(verify)*

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Classic mirrored queues were deprecated and then removed. Why did they fall out of favour, and how would you migrate a live estate to quorum queues without losing messages? *(verify)*
- [ ] What do quorum queues cost in memory, disk and throughput, which classic queue features do they lack or handle differently, and when is a non-replicated classic queue still the right call? *(verify)*
- [ ] What does a RabbitMQ cluster replicate across nodes and what does it not? Cover metadata, message data and what happens when a client connects to a node that does not host the queue leader. *(verify)*
- [ ] Partition handling: compare `pause_minority`, `autoheal` and `ignore` on consistency and availability. Which would you pick for three availability zones, and why is a two-zone cluster a trap? *(verify)*
- [ ] At-most-once vs. at-least-once dead-lettering: how can a message be lost between a queue and its DLQ, and what must be configured on a quorum queue to prevent that? *(verify)*
- [ ] How does internal credit-based flow control differ from a memory or disk alarm, what does `connection.blocked` tell a client, and why should publishing and consuming not share a connection?
- [ ] A single queue is limited by one Erlang process. How do you scale past that with a consistent hash exchange or super streams, and what happens to per-key ordering? *(verify)*
- [ ] Fan-out to twenty consumers with replay: one queue per consumer or one stream? Compare storage, ordering, consumer lag and what each makes hard.
- [ ] Federation vs. Shovel vs. stretching a cluster across regions: what does each replicate, how does each fail, and which would you use for multi-region messaging?
- [ ] How do you upgrade a production cluster: rolling restart with feature flags, or blue-green with a second cluster? What constrains version jumps, and how do clients behave during the move? *(verify)*

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] One node of a three-node cluster is cut off from the others while publishers stay connected to it. What happens to classic and quorum queues, and what do you check after the network heals?
- [ ] A queue is growing, every consumer is connected, and the management UI shows unacked count equal to prefetch times consumers with no progress. How do you find the cause and recover?
- [ ] Design the exchange, queue and dead-letter topology for order events consumed by five teams, with retries at 1 minute, 10 minutes and 1 hour. How do naming, ownership and ordering work?
- [ ] An order platform needs per-message routing and retries, plus 90-day replay for analytics. Would you use RabbitMQ with streams, Kafka, or both, and do Kafka share groups change your answer? *(verify)*
- [ ] An organisation runs a large JMS estate on ActiveMQ Classic using selectors and XA transactions. When would you keep it, move to ActiveMQ Artemis, or move to RabbitMQ? *(verify)*
- [ ] Core NATS vs. JetStream: what delivery guarantees does each give, and when would NATS beat RabbitMQ or Kafka, for example on request-reply, edge deployments or operational footprint? *(verify)*
- [ ] Pulsar separates stateless brokers from BookKeeper storage. What does that give you on multi-tenancy, geo-replication and rebalancing compared with Kafka and RabbitMQ, and what does it cost to operate? *(verify)*
- [ ] Build a decision table for RabbitMQ, Kafka, ActiveMQ, NATS and Pulsar covering routing, replay, ordering, ecosystem, licence and managed availability. Which criteria dominate for a five-person team compared with a platform group? *(verify)*

## E. Hands-on checks (things to be able to DO)
- [ ] Define a topology in a definitions file with a topic exchange, an alternate exchange and a dead-letter exchange, then publish an unroutable message and show where it lands.
- [ ] Write a publisher using asynchronous confirms and `mandatory` returns, restart the broker mid-run, and reconcile published, confirmed and consumed counts.
- [ ] Use PerfTest to measure throughput and latency at prefetch 1, 10, 100 and 1000, then explain the shape of the results. *(verify)*
- [ ] Run a three-node cluster in Docker Compose with a quorum queue, kill the leader to show failover, then isolate a node under `pause_minority` and observe what clients see.
- [ ] Build tiered retries with TTL and dead-lettering that read `x-death`, then consume a stream from the first message, from a timestamp and from a stored offset.

---

**Good answers show:**
- Tracing a message through exchange, binding, queue and consumer, and naming exactly where it can be dropped at each step.
- Safety treated as a chain: durable queue, persistent message, publisher confirm, `mandatory` and manual ack, with a clear account of what each one leaves open.
- Failure-mode thinking on quorum loss, partitions, resource alarms and leader moves rather than assuming the cluster simply stays up.
- Tuning from measurement: prefetch, queue length and confirm latency justified by observed numbers, not copied defaults.
- Retries and dead-lettering designed as one system, including what ordering is given up and who owns the DLQ.
- A broker choice argued from the workload and the team's operational capacity, with an honest statement of what would change the recommendation.

**Prove it:** Build a small order-events system on a three-node RabbitMQ cluster with quorum queues, publisher confirms, tiered retries, a DLQ and a stream for replay. Inject a leader kill, a network partition and a disk alarm during a load test, then record losses, duplicates and recovery time for each. Finish with a one-page decision record comparing the same workload on Kafka and NATS JetStream, using at least one measured run on either.
