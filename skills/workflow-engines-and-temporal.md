# Workflow engines & Temporal

Any business process that spans several services and takes longer than a request eventually needs retries, timers and a way to recover from a half-finished state. Interviewers use this topic to test whether you can reason about partial failure rather than assume the happy path, and whether you know when a table and a cron job are enough. Senior backend, Software Architect and Forward Deployed Engineer roles ask most, particularly on products with payments, onboarding or customer-defined automations.

> Related: [RabbitMQ, SQS & task queues](rabbitmq-sqs-and-task-queues.md) · [Kafka & event streaming](kafka-and-event-streaming.md) · [Large-scale distributed systems](large-scale-distributed-systems.md) · [System design](../topics/03-system-design/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] What does durable execution mean, and what problem does it solve that a queue and a status column do not?
- [ ] In Temporal, what are workflows, activities, workers and task queues, and where does your code actually run?
- [ ] Why must Temporal workflow code be deterministic, and which common operations break that rule?
- [ ] How does Temporal recover a workflow after a worker crashes? Explain event history and replay.
- [ ] What is a saga, and how does it differ from a distributed transaction?
- [ ] What is a compensating action, and why is it not the same as a rollback?
- [ ] What problem does the transactional outbox pattern solve, and what delivery guarantee does it give you?
- [ ] Orchestration vs. choreography: how do they differ, and how do you choose?
- [ ] AWS Step Functions: what is a state machine, and how do Standard and Express workflows differ? *(verify)*

## B. Working depth
- [ ] Temporal activity timeouts: what do schedule-to-close, start-to-close, schedule-to-start and heartbeat each protect against? *(verify)*
- [ ] How do activity retries interact with idempotency? How would you make a payment activity safe to run twice?
- [ ] Signals vs. queries vs. updates in Temporal: when would you use each to interact with a running workflow? *(verify)*
- [ ] Why does Temporal limit event history size, and how does continue-as-new work for long-running workflows? *(verify)*
- [ ] When would you use a child workflow rather than an activity or a separate workflow?
- [ ] Step Functions: how do retry and catch, Map states and the `waitForTaskToken` callback pattern work? *(verify)*
- [ ] How do you wait for a human approval that may take 30 days, including reminders, escalation and timeout?
- [ ] Outbox via polling vs. CDC: what are the differences in ordering and latency, and how do consumers deduplicate?
- [ ] How do you order saga steps, and why should steps that cannot be compensated come as late as possible?
- [ ] What should a compensation do when it fails too, or when the original step's effect is still in doubt?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] How do you change workflow code while thousands of executions are in flight? Compare patching, worker versioning and starting new versions side by side. *(verify)*
- [ ] Ten million workflows are sleeping until midnight on the same date. How do durable timers work, and how do you avoid a thundering herd when they all fire?
- [ ] How do you build scheduled waits without an engine, using a database table, delay queue or timer wheel, and where do those approaches stop scaling?
- [ ] Temporal Cloud vs. self-hosted: what does running the service and its database involve, and how do you compare the cost? *(verify)*
- [ ] Temporal vs. Step Functions vs. Airflow: which kinds of work does each suit, and what are the costs of the wrong choice? *(verify)*
- [ ] Build vs. buy: when is a home-grown state machine on PostgreSQL the right decision, and which features do teams always underestimate?
- [ ] Sagas provide no isolation between steps. Which anomalies can other requests observe, and how do semantic locks or pending states help?
- [ ] How do you give operators visibility into stuck workflows, and how do you safely retry, skip a step, or repair thousands of them after a bad deploy?
- [ ] What are the signs that a workflow engine is becoming a bottleneck (task queue backlog, worker slots, history shards), and how do you scale it? *(verify)*
- [ ] How do you test long-running workflows: time skipping, replay tests against production histories, and failure injection? *(verify)*
- [ ] Multi-tenant fairness: how do you stop one customer's bulk workflow starts from delaying everyone else's?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Design an order flow that reserves stock, takes payment and arranges shipping, with compensation when shipping fails after the payment has succeeded.
- [ ] Design a customer-facing automation engine, like a marketing or CRM workflow builder, with triggers, branches, delays and actions defined by non-engineers.
- [ ] A customer edits an automation while 200,000 contacts are part-way through it. What should happen, and how do you model versions to make that possible?
- [ ] A deploy causes non-determinism errors, and workflows have stopped making progress. How do you diagnose and recover without losing state?
- [ ] An activity timed out after the third-party API actually succeeded, and the customer was charged twice. Walk through the cause and the fix.
- [ ] Your team has a tangle of cron jobs, status columns and retry scripts for onboarding. Make the case for or against adopting Temporal, and describe the migration.
- [ ] Design a subscription dunning process with retries over 30 days, customer emails, plan changes mid-process, and cancellation.

## E. Hands-on checks (things to be able to DO)
- [ ] Run Temporal locally, write a workflow with two activities, kill the worker mid-run, and show it resuming from where it stopped. *(verify)*
- [ ] Trigger a non-determinism failure by changing workflow code, then fix it with the SDK's patching or versioning API. *(verify)*
- [ ] Implement a saga with compensation, and write tests that inject a failure at every step.
- [ ] Write the same approval process as a Step Functions state machine and as a Temporal workflow, then compare them.
- [ ] Build a transactional outbox with an idempotent consumer, and show correct results after killing the relay mid-batch.
- [ ] Build a durable timer table on PostgreSQL using `FOR UPDATE SKIP LOCKED`, and measure how late timers fire under load.

---

**Good answers show:**
- Reasoning from partial failure: for each step, what happens if the process dies just before or just after the side effect.
- Idempotency and compensation designed per step, including steps that cannot be undone and compensations that fail.
- Clear understanding of replay and determinism, and of how code changes interact with running executions.
- Long-running concerns taken seriously: versioning, timers at scale, history growth and fairness.
- Operability: finding, repairing and explaining stuck workflows, not only running healthy ones.
- A balanced build vs. buy judgement, including when a simple state machine in the database is enough.

**Prove it:** Build a small customer-defined automation engine with triggers, branches, delays and actions on Temporal, then rebuild its core on PostgreSQL with an outbox and a timer table. Run 100,000 executions while killing workers, and change a workflow definition mid-run. Write up the measured timer lateness, duplicate side effects and operational effort for each approach, and say which you would choose and why.
