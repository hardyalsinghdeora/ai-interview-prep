# Study Plan — Software Development Engineer (Backend)

For engineers preparing for a backend SDE loop at a technology company, SaaS vendor or startup; assumes about 10–12 hours a week for six weeks.

Read the role overview first: [README.md — The role](./README.md#the-role) and the [typical interview loop](./README.md#typical-interview-loop). SDE loops centre on coding, then add low-level design, backend fundamentals, system design from mid level up, and behavioral questions. This plan keeps coding practice running every week and adds one backend area at a time, and you build a small service along the way so that your answers come from practice.

## Before you start

Spend 30 minutes rating yourself honestly. Skip or skim areas you rate 4 or 5.

| Skill area | Rate yourself 1–5 | What a 4 looks like |
|---|---|---|
| DS&A coding | | You solve most medium problems in 25–30 minutes, speak throughout and state complexity without prompting. |
| Your main language | | You know its collections, concurrency model, error handling and testing tools well enough to write them without searching. |
| Low-level design | | In 60–90 minutes you turn a loose brief into working classes with clear interfaces and a few tests. |
| APIs | | You can design resources, errors, pagination, idempotency and versioning, and explain each choice. |
| Databases | | You can read a query plan, choose composite indexes, explain isolation levels and plan a zero-downtime migration. |
| Caching and messaging | | You can explain cache-aside, stampedes, the outbox pattern, idempotent consumers and dead-letter queues. |
| Concurrency | | You can explain threads versus async, prevent deadlocks and diagnose pool exhaustion. |
| System design | | In 45 minutes you reach a reasoned design with a data model, a capacity estimate and failure handling. |
| Testing and production | | You can write a test strategy, debug a latency regression and say what telemetry a service needs. |
| Behavioral | | You have six stories with your own actions and measured results. |

## 6-week plan

Assumes ~10–12 hours a week. Use one language for everything, ideally the one you use at work or the one the employer prefers. Do at least four timed coding problems every week, whatever the week's theme.

### Week 1 — Coding patterns and your language

**Goals:** a reliable coding routine, and quick recall of your language's tools.

- [ ] Read [How do you approach a 45-minute coding interview?](./README.md#how-do-you-approach-a-45-minute-coding-interview)
- [ ] Work [DS&A topics — Arrays & strings](../../topics/02-coding/ANSWERS-dsa-topics.md#arrays--strings), [Hashing](../../topics/02-coding/ANSWERS-dsa-topics.md#hashing) and [Stacks & queues](../../topics/02-coding/ANSWERS-dsa-topics.md#stacks--queues).
- [ ] Review the [Data structures & algorithms cheatsheet](../../cheatsheets/data-structures-and-algorithms.md).
- [ ] Drill the fundamentals and working-depth levels of your language's list: [Java & Spring Boot](../../skills/java-and-spring-boot.md), [Python](../../skills/python.md), [Go](../../skills/go.md), [Node.js & TypeScript](../../skills/nodejs-and-typescript.md), [.NET & C#](../../skills/dotnet-and-csharp.md) or [PHP & Laravel](../../skills/php-and-laravel.md).
- [ ] Read [What changes in expectations between a junior, mid-level and senior backend engineer?](./README.md#what-changes-in-expectations-between-a-junior-mid-level-and-senior-backend-engineer)

**Deliverable:** a public repo with an [LRU cache](./README.md#implement-an-lru-cache-and-make-it-safe-to-use-from-several-threads) and a [token bucket](./README.md#implement-a-token-bucket-rate-limiter), written from memory, with unit tests.

**Mock:** a 45-minute coding screen with a peer who stays quiet unless you ask a question.

**End of week check:** you can name the likely pattern for a new problem within five minutes, and test your solution before being asked.

### Week 2 — More coding, and low-level design

**Goals:** cover trees, graphs and dynamic programming, and start producing working designs.

- [ ] Work [Trees](../../topics/02-coding/ANSWERS-dsa-topics.md#trees), [Graphs](../../topics/02-coding/ANSWERS-dsa-topics.md#graphs), [Binary search](../../topics/02-coding/ANSWERS-dsa-topics.md#binary-search) and [Dynamic programming](../../topics/02-coding/ANSWERS-dsa-topics.md#dynamic-programming).
- [ ] Read [Low-level design — model answers](./README.md#low-level-design-1).
- [ ] Work two problems from [02-coding section C](../../topics/02-coding/ANSWERS.md#c-object-oriented--low-level-design-code-it), such as a [parking lot](../../topics/02-coding/ANSWERS.md#parking-lot) and a [state machine engine](../../topics/02-coding/ANSWERS.md#simple-workflowstate-machine-engine-eg-order-lifecycle).
- [ ] Drill [OOP, design patterns & LLD](../../skills/oop-design-patterns-and-lld.md) and skim the [OOP & design patterns cheatsheet](../../cheatsheets/oop-and-design-patterns.md).

**Deliverable:** one machine coding exercise in working code, finished within 90 minutes, with a short note on how a new requirement would fit.

**Mock:** a 60-minute low-level design round where a peer adds a new requirement at the 40-minute mark.

**End of week check:** you start with interfaces and a working core flow rather than spending half the time on diagrams.

### Week 3 — APIs and databases

**Goals:** show real depth in the area that sets backend engineers apart.

- [ ] Read [API and service design — model answers](./README.md#api-and-service-design-1) and [Databases — model answers](./README.md#databases-1).
- [ ] Study [07 — SQL concepts](../../topics/07-data-and-sql/ANSWERS.md#b-sql-concepts), [Data modelling](../../topics/07-data-and-sql/ANSWERS.md#c-data-modeling) and [13 — Databases](../../topics/13-cs-fundamentals/ANSWERS.md#databases).
- [ ] Drill [API design & microservices](../../skills/api-design-and-microservices.md) and [PostgreSQL & data modelling](../../skills/postgresql-and-data-modelling.md) or [MySQL](../../skills/mysql.md).
- [ ] Skim the [API design](../../cheatsheets/api-design.md) and [Databases & SQL](../../cheatsheets/databases-and-sql.md) cheatsheets.

**Deliverables:**
1. Start a small order service with a REST API, idempotent creation, cursor pagination and a relational database. Keep the code in a public repo.
2. Load a million rows, find a slow query with `EXPLAIN`, and record the plan before and after adding an index.

**Mock:** a 45-minute backend fundamentals round: a peer asks about isolation levels, indexing and [overselling the last item](./README.md#two-customers-try-to-buy-the-last-item-at-the-same-moment-how-do-you-prevent-overselling).

**End of week check:** you can describe an [expand-and-contract migration](./README.md#how-do-you-rename-a-column-or-change-a-schema-on-a-live-table-without-downtime) step by step, including why each step is safe during a rolling deployment.

### Week 4 — Caching, messaging and concurrency

**Goals:** handle the failure modes of systems with more than one moving part.

- [ ] Read [Caching and messaging — model answers](./README.md#caching-and-messaging-1) and [Concurrency and runtime — model answers](./README.md#concurrency-and-runtime-1).
- [ ] Study [13 — Operating systems](../../topics/13-cs-fundamentals/ANSWERS.md#operating-systems) and [Distributed systems](../../topics/13-cs-fundamentals/ANSWERS.md#distributed-systems).
- [ ] Drill [Redis & caching](../../skills/redis-and-caching.md), and one of [Kafka & event streaming](../../skills/kafka-and-event-streaming.md) or [RabbitMQ, SQS & task queues](../../skills/rabbitmq-sqs-and-task-queues.md).
- [ ] Skim the [Distributed systems](../../cheatsheets/distributed-systems.md), [Messaging & streaming](../../cheatsheets/messaging-and-streaming.md) and [CS fundamentals](../../cheatsheets/cs-fundamentals.md) cheatsheets.

**Deliverable:** add to the order service: a read cache, a transactional outbox, a consumer that removes duplicates, and a dead-letter queue. Add a test that delivers the same event twice.

**Mock:** a 30-minute debugging discussion: "[connection pool exhausted](./README.md#requests-are-timing-out-with-connection-pool-exhausted-what-do-you-check)", then "[consumer lag keeps growing](../../topics/12-debugging-troubleshooting/ANSWERS.md#kafka-consumer-lag-keeps-growing)".

**End of week check:** you can explain why a database write followed by a broker publish is unsafe, and what the consumer must do under at-least-once delivery.

### Week 5 — System design, testing and production

**Goals:** a repeatable design method, and an operator's view of your own service.

- [ ] Read [System design for backend engineers — model answers](./README.md#system-design-for-backend-engineers-1) and [Testing, security and production — model answers](./README.md#testing-security-and-production-1).
- [ ] Work prompts from [03 — Classic system design](../../topics/03-system-design/ANSWERS.md#c-classic-system-design-still-asked): [URL shortener](../../topics/03-system-design/ANSWERS.md#url-shortener), [rate limiter](../../topics/03-system-design/ANSWERS.md#rate-limiter-distributed) and [notification system](../../topics/03-system-design/ANSWERS.md#notification-system-emailsmspush), plus the [follow-up questions](../../topics/03-system-design/ANSWERS.md#d-follow-up-questions-to-expect).
- [ ] Study [12 — Production troubleshooting scenarios](../../topics/12-debugging-troubleshooting/ANSWERS.md#b-production-troubleshooting-scenarios).
- [ ] Drill [System design](../../skills/system-design.md), [Testing & quality](../../skills/testing-and-quality.md), [Observability & reliability](../../skills/observability-and-reliability.md), [Docker & CI/CD](../../skills/docker-and-ci-cd.md) and [Authentication & authorisation](../../skills/authentication-and-authorization.md).

**Deliverables:**
1. Add integration tests, metrics, structured logs, a Dockerfile and a CI pipeline to the order service.
2. Two system design write-ups, each with an API, a data model, a capacity estimate and failure handling.

**Mock:** a 45-minute system design of the [webhook delivery service](./README.md#design-a-webhook-delivery-service), going deep on retries and isolation between endpoints.

**End of week check:** you can do the capacity estimate out loud and say what the numbers mean for the design.

### Week 6 — Project deep dive, behavioral and full loop

**Goals:** tell your project and your stories clearly, then rehearse the whole loop.

- [ ] Prepare [the project walkthrough](./README.md#walk-me-through-a-backend-system-you-built-or-changed-what-would-you-do-differently).
- [ ] Read [Behavioral — model answers](./README.md#behavioral-1) and [Engineering practice — model answers](./README.md#engineering-practice-1).
- [ ] Study [10 — Ownership & impact](../../topics/10-behavioral/ANSWERS.md#ownership--impact), [Conflict & collaboration](../../topics/10-behavioral/ANSWERS.md#conflict--collaboration), [Failure & learning](../../topics/10-behavioral/ANSWERS.md#failure--learning) and [Problem solving & technical judgement](../../topics/10-behavioral/ANSWERS.md#problem-solving--technical-judgment).
- [ ] Practise [Tier 1 of COMMON-QUESTIONS.md](../COMMON-QUESTIONS.md#tier-1--asked-in-almost-every-loop), skipping the customer-work items.
- [ ] Prepare questions: [14 — To an engineer interviewer](../../topics/14-questions-to-ask/ANSWERS.md#to-an-engineer-interviewer) and [to the hiring manager](../../topics/14-questions-to-ask/ANSWERS.md#to-the-hiring-manager).
- [ ] Tick off the role's [Prep checklist](./README.md#prep-checklist).

**Deliverable:** a story matrix: rows are ownership, failure, disagreement, deadline trade-off, fast learning and helping a teammate; each row has one real story with your actions and a measured result.

**Mock:** a full mock loop: coding (45), low-level design (60), system design (45) and a behavioral round where the interviewer keeps asking "what exactly did you do?" (45).

**End of week check:** you can finish a coding round with tested code, and every story survives three follow-up questions.

## 2-week crash plan

| Day | Focus | Tasks | Links |
|---|---|---|---|
| 1 | Role and method | Read the role; two arrays and hashing problems | [README — role](./README.md#the-role), [Coding approach](./README.md#how-do-you-approach-a-45-minute-coding-interview), [Arrays & strings](../../topics/02-coding/ANSWERS-dsa-topics.md#arrays--strings) |
| 2 | Coding | Trees and graphs; LRU cache from memory | [Trees](../../topics/02-coding/ANSWERS-dsa-topics.md#trees), [Graphs](../../topics/02-coding/ANSWERS-dsa-topics.md#graphs), [Coding round answers](./README.md#coding-round-1) |
| 3 | Low-level design | One design in working code | [Low-level design answers](./README.md#low-level-design-1), [02 — Section C](../../topics/02-coding/ANSWERS.md#c-object-oriented--low-level-design-code-it) |
| 4 | APIs | Order API, idempotency, pagination | [API answers](./README.md#api-and-service-design-1), [API design cheatsheet](../../cheatsheets/api-design.md) |
| 5 | Databases | Indexes, isolation, overselling, migrations | [Database answers](./README.md#databases-1), [Databases & SQL cheatsheet](../../cheatsheets/databases-and-sql.md) |
| 6 | Caching and messaging | Cache-aside, outbox, consumers | [Caching and messaging answers](./README.md#caching-and-messaging-1) |
| 7 | Mock day | Coding mock + backend fundamentals mock | [Mock schedule](#mock-interview-schedule) |
| 8 | Concurrency | Threads versus async; deadlocks; pool exhaustion | [Concurrency answers](./README.md#concurrency-and-runtime-1), [13 — Operating systems](../../topics/13-cs-fundamentals/ANSWERS.md#operating-systems) |
| 9 | System design | Method and estimate; one classic prompt | [System design answers](./README.md#system-design-for-backend-engineers-1), [03 — Section C](../../topics/03-system-design/ANSWERS.md#c-classic-system-design-still-asked) |
| 10 | System design | Webhook delivery; follow-up questions | [Webhook answer](./README.md#design-a-webhook-delivery-service), [03 — Section D](../../topics/03-system-design/ANSWERS.md#d-follow-up-questions-to-expect) |
| 11 | Production | Latency regression; telemetry; rollout | [Production answers](./README.md#testing-security-and-production-1), [12 — Section B](../../topics/12-debugging-troubleshooting/ANSWERS.md#b-production-troubleshooting-scenarios) |
| 12 | Project deep dive | Rehearse the 20-minute walkthrough | [Walkthrough answer](./README.md#walk-me-through-a-backend-system-you-built-or-changed-what-would-you-do-differently) |
| 13 | Behavioral | Story matrix; full mock loop | [Behavioral answers](./README.md#behavioral-1), [story bank checklist](../../topics/00-overview/README.md#story-bank-checklist-prepare-one-story-for-each) |
| 14 | Rest and review | Light revision; redo two earlier problems | [README checklist](./README.md#prep-checklist) |

## Daily routine

A realistic 90-minute weekday:

| Minutes | Activity |
|---|---|
| 0–40 | One coding problem, timed and spoken out loud. Write tests. |
| 40–50 | Compare with a model solution, and note the pattern you missed. |
| 50–75 | The week's topic: one README model answer, then a sibling question without notes. |
| 75–90 | Five questions from a skill list, or one behavioral story out loud. |

Weekends: build the order service, and do the week's mock.

## Mock interview schedule

| Week | Mock | With whom |
|---|---|---|
| 1 | Coding screen | A peer engineer who stays quiet unless asked |
| 2 | Low-level design with a new requirement | A senior engineer, or a peer who has done machine coding rounds |
| 3 | Backend fundamentals | A backend engineer; interviewing.io and Exponent offer technical mocks |
| 4 | Debugging discussion | A peer who has been on-call |
| 5 | System design | A senior engineer who pushes on data model and failure handling |
| 6 | Full loop | Two or three partners, including one running the behavioral round |

Do at least one coding mock every week from Week 2, and record one behavioral answer a week.

## Resources

- *Designing Data-Intensive Applications* by Martin Kleppmann.
- *System Design Interview* (volumes 1 and 2) by Alex Xu.
- *Cracking the Coding Interview* by Gayle Laakmann McDowell.
- NeetCode's problem lists and LeetCode, for timed practice.
- *Use The Index, Luke!* (https://use-the-index-luke.com/) for SQL indexing.
- *Release It!* by Michael Nygard, for production failure patterns.
- *A Philosophy of Software Design* by John Ousterhout.
- OWASP API Security Top 10 (https://owasp.org/API-Security/).
- The official documentation for your database and language.
