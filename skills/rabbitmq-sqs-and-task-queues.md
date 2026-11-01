# RabbitMQ, SQS & task queues

Most background work in a backend runs on a queue, not a log, and interviewers use this topic to test whether you understand acknowledgements, redelivery and retries well enough to avoid double-charging a customer at 3am. Senior backend, Solutions Architect and Forward Deployed Engineer roles ask most, often as "why not just use Kafka?" or by handing you a queue that has stopped draining.

> Related: [Kafka & event streaming](kafka-and-event-streaming.md) · [Workflow engines & Temporal](workflow-engines-and-temporal.md) · [System design](../topics/03-system-design/README.md) · [Cloud, DevOps & Integration](../topics/08-cloud-devops-integration/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Queue vs. log: what happens to a message after it is consumed in each, and what does that mean for replay and fan-out?
- [ ] In RabbitMQ, how does a message get from publisher to consumer through an exchange, binding, routing key and queue?
- [ ] Direct vs. topic vs. fanout vs. headers exchanges — give one real use for each.
- [ ] Ack, nack, reject and requeue: what does each do, and what happens to unacked messages when a consumer's connection drops?
- [ ] SQS standard vs. FIFO: how do delivery, ordering and throughput differ, and how do you pick? *(verify)*
- [ ] What is a visibility timeout, and what happens if processing takes longer than it?
- [ ] What is a dead-letter queue, and what should send a message there?
- [ ] Why does adding competing consumers weaken ordering, even on a queue that delivers in order?
- [ ] Why must a consumer on an at-least-once queue be idempotent? Give a concrete example of a side effect that makes this hard.
- [ ] Google Pub/Sub vs. Cloud Tasks: who controls delivery timing and rate in each, and when would you pick one over the other? *(verify)*

## B. Working depth
- [ ] What does prefetch (`basic.qos`) control in RabbitMQ, and what goes wrong when it is set too high or too low?
- [ ] Publisher confirms, durable queues and persistent messages: what does each protect against, and what can still be lost if you only use one?
- [ ] How do you run a job longer than the visibility timeout? When would you extend it with a heartbeat rather than simply raising it?
- [ ] SQS FIFO: what do message group ID and deduplication ID each do, and how does one slow message affect the rest of its group? *(verify)*
- [ ] How do you implement backoff with jitter in RabbitMQ (TTL plus dead-letter exchange), SQS (visibility changes or delay) and a BullMQ-style library?
- [ ] Pub/Sub: how do ack deadlines, ordering keys, push vs. pull subscriptions and dead-letter topics fit together? *(verify)*
- [ ] Cloud Tasks: how do task names, schedule time and per-queue rate limits help when you call a fragile downstream API? *(verify)*
- [ ] BullMQ: how are jobs held in Redis, what is a stalled job, and which Redis settings can silently lose your queue? *(verify)*
- [ ] Celery: what do `acks_late`, the prefetch multiplier and the broker visibility timeout change, and why can long ETA tasks run twice on Redis or SQS brokers? *(verify)*
- [ ] Lambda consuming from SQS: how do batch size, partial batch failures and reserved concurrency interact? *(verify)*

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] When is Kafka the wrong choice? Argue from per-message retries, delayed delivery, low volume and the team's operational capacity.
- [ ] How do you get per-customer ordering and parallelism at once, and what do you give up when one customer's message keeps failing?
- [ ] RabbitMQ classic vs. quorum queues vs. streams: durability, failover behaviour and when each fits. *(verify)*
- [ ] Backpressure: what happens to publishers when RabbitMQ hits memory or disk alarms, and how should a producer behave when a queue cannot keep up?
- [ ] Scale workers on queue depth or on age of the oldest message? Why does the choice matter, and how do you scale in without killing in-flight jobs?
- [ ] One large customer enqueues a million jobs and everyone else waits. Compare per-tenant queues, weighted fair scheduling and rate limits at enqueue.
- [ ] How do you operate a DLQ in production: alerting, ownership, inspection, safe redrive, and the retention traps that can expire messages early? *(verify)*
- [ ] Where do "exactly-once" claims from SQS FIFO deduplication or Pub/Sub exactly-once delivery stop applying? *(verify)*
- [ ] Priority queues and delayed jobs: how do you avoid starving low-priority work, and why do many delayed messages strain some brokers?
- [ ] Compare the cost and ops burden of SQS, a RabbitMQ cluster, a Redis-backed queue and Pub/Sub at low and at high volume. *(verify)*

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Customers are receiving duplicate invoices. The visibility timeout is 30 seconds and the job sometimes takes two minutes. Walk through the cause, the immediate mitigation and the lasting fix.
- [ ] A worker runs out of memory on one particular message, which is redelivered forever and takes the fleet down with it. How do you stop it and prevent a repeat?
- [ ] Design a document-processing job system with priorities, progress reporting, cancellation and per-customer limits.
- [ ] A team proposes Kafka for background jobs at 50 messages per second. What do you ask, what do you recommend, and what would change your mind?
- [ ] A downstream outage has left two million messages in the queue. How do you recover without stampeding the service that has just come back?
- [ ] Design outbound calls to a partner API limited to 10 requests per second per customer, with retries that respect `Retry-After`.
- [ ] Migrate from RabbitMQ to SQS (or the reverse) with no lost jobs, and explain how you cut over while old messages are still in flight.

## E. Hands-on checks (things to be able to DO)
- [ ] Run RabbitMQ in Docker, route messages through a topic exchange to two queues, and show unacked messages returning when you kill a consumer.
- [ ] Build retries with exponential backoff and a final dead-letter queue, then show a poison message reaching it after the expected number of attempts.
- [ ] Reproduce a duplicate delivery on SQS or LocalStack by exceeding the visibility timeout, then make the handler idempotent with a PostgreSQL unique constraint.
- [ ] Set up a BullMQ or Celery worker with concurrency, rate limiting and graceful shutdown, and show that no job is lost during a deploy.
- [ ] Write a safe DLQ redrive script with a rate limit and a dry-run mode.
- [ ] Build a dashboard showing queue depth, age of oldest message, processing rate and retry count, with an alert you would actually want to be woken by.

---

**Good answers show:**
- A clear rule for choosing a queue or a log, tied to replay, fan-out, per-message retries and ops cost.
- An assumption of at-least-once delivery, with idempotency designed into the side effect rather than hoped for from the broker.
- Honest treatment of ordering: which key it applies to, and what a failing message does to everything behind it.
- Retries designed as a whole: backoff, jitter, attempt limits, a DLQ and a plan for what happens to what lands in it.
- Operational instincts: age of oldest message over raw depth, backpressure, graceful shutdown and recovery without a stampede.
- Fairness and blast radius thinking, so one customer or one bad message cannot stall everyone.

**Prove it:** Build a small job service with an API that enqueues work and a pool of idempotent workers, first on RabbitMQ or BullMQ and then on SQS. Add backoff, a DLQ with a redrive tool, per-customer rate limits and autoscaling on the age of the oldest message. Kill workers and the downstream dependency during a load test, then write up the measured duplicates, losses and recovery time on each broker.
