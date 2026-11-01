# Software Development Engineer (Backend)

An engineer who designs, builds, tests and runs server-side software: APIs, business logic, data stores and background processing. The job is to ship correct code that keeps working in production, usually as part of a product team and with little direct customer contact.

## The role

**Day to day.** Picking up scoped work from a team backlog; writing code and tests; designing APIs and database schemas; reviewing teammates' pull requests; writing short design notes for larger changes; deploying behind flags and watching the dashboards; taking an on-call shift and fixing what it reveals; and working with product managers, front-end engineers and other backend teams on interfaces and dates.

**Common titles.** Software Development Engineer (SDE I, II, III at Amazon and companies that copy its ladder), Software Engineer (Google's L3 to L5, Meta's E3 to E5) *(verify)*, Backend Engineer, Server-side Engineer, Member of Technical Staff (MTS), and Senior Software Engineer. The same title can sit at different levels at different companies, so compare scope and years of experience rather than the name.

**Who hires for it.** Almost every software company: large technology companies, SaaS vendors, banks and fintechs, e-commerce and logistics companies, and startups. Loop details below are typical or reported, not guaranteed.

**How it differs from neighbouring roles.**

| | Backend SDE | [Forward Deployed Engineer](../01-forward-deployed-engineer/README.md) | [Founding / Product Engineer](../17-founding-product-engineer/README.md) | [Tech Lead](../19-tech-lead/README.md) |
|---|---|---|---|---|
| Main output | Reliable services and features | Customer deployments and integrations | The whole product, end to end | A team's delivery and technical direction |
| Who defines the work | Team backlog and product manager | The customer's problem | Founders and users | Tech lead with product and EM |
| Customer contact | Low | Daily | Frequent | Low to medium |
| Depth vs breadth | Deep in one stack and domain | Broad, adapts to each customer | Very broad | Broad across the team's systems |
| Coding share | Most of the week | Most of the week | Most of the week | Roughly a third to a half |

**Who it suits.** People who enjoy solving precise technical problems, care how code behaves under load and failure, and are happy for the code itself to be the main output.

**Skills mix.**

| Skill | Weight |
|---|---|
| Coding and data structures | High |
| Backend fundamentals (APIs, databases, concurrency) | High |
| Low-level design and clean code | High |
| System design | Med (High from senior level) |
| Testing, debugging and production operations | Med to High |
| Communication and collaboration | Med |
| Customer or commercial skills | Low |

**Career paths.** SDE to Senior SDE, then either the individual contributor track (Staff, Principal) or leadership ([Tech Lead](../19-tech-lead/README.md), [Engineering Manager](../20-engineering-manager/README.md)). Sideways moves include platform or site reliability engineering, data engineering, and [Forward Deployed Engineer](../01-forward-deployed-engineer/README.md).

## Typical interview loop

Typical reported shape; varies by company and level. Junior loops lean on coding, and senior loops add system design and a deeper project discussion.

| Round | What it tests | Prepare with |
|---|---|---|
| Recruiter screen | Background, level, motivation, logistics | [01-recruiter-and-motivation](../../topics/01-recruiter-and-motivation/ANSWERS.md) |
| Online assessment (common for junior and mid levels) | Timed DS&A problems, sometimes debugging or SQL | [DS&A topics](../../topics/02-coding/ANSWERS-dsa-topics.md) |
| Technical phone screen | One or two coding problems, talking through your approach | This file + [02-coding](../../topics/02-coding/ANSWERS.md) |
| Coding rounds (one to three) | Problem solving, correctness, complexity, code quality, testing | This file + [DS&A topics](../../topics/02-coding/ANSWERS-dsa-topics.md) |
| Low-level design or machine coding (*reported* as common at product companies in India, and increasingly elsewhere) | Classes, interfaces, extensibility, working code in 60–90 minutes | This file + [02-coding section C](../../topics/02-coding/ANSWERS.md#c-object-oriented--low-level-design-code-it) |
| System design (mid level and above) | Requirements, data model, scaling, failure handling, trade-offs | This file + [03-system-design](../../topics/03-system-design/ANSWERS.md) |
| Backend fundamentals or project deep dive | Databases, concurrency, APIs, and depth on something you built | This file + [13-cs-fundamentals](../../topics/13-cs-fundamentals/ANSWERS.md) |
| Behavioral / hiring manager (at Amazon, structured around Leadership Principles with a "Bar Raiser" interviewer *(verify)*) | Ownership, collaboration, learning from mistakes | This file + [10-behavioral](../../topics/10-behavioral/ANSWERS.md) |

## Common rounds — use the shared sections

The shared sections were written with customer-facing roles in mind. The technical content applies directly; skip questions about customers, travel and on-site work unless the job description mentions them.

- [02 Coding](../../topics/02-coding/ANSWERS.md) and [DS&A topics](../../topics/02-coding/ANSWERS-dsa-topics.md): highest priority. Section C covers object-oriented design problems, and section D covers the follow-up questions asked after coding.
- [03 System design](../../topics/03-system-design/ANSWERS.md): [classic prompts](../../topics/03-system-design/ANSWERS.md#c-classic-system-design-still-asked) and [follow-up questions](../../topics/03-system-design/ANSWERS.md#d-follow-up-questions-to-expect).
- [13 CS fundamentals](../../topics/13-cs-fundamentals/ANSWERS.md): networking, operating systems, databases, distributed systems and software engineering practices.
- [07 Data & SQL](../../topics/07-data-and-sql/ANSWERS.md): [SQL concepts](../../topics/07-data-and-sql/ANSWERS.md#b-sql-concepts) and [data modelling](../../topics/07-data-and-sql/ANSWERS.md#c-data-modeling).
- [08 Cloud, DevOps & integration](../../topics/08-cloud-devops-integration/ANSWERS.md): [APIs](../../topics/08-cloud-devops-integration/ANSWERS.md#a-apis--integration), [authentication and security](../../topics/08-cloud-devops-integration/ANSWERS.md#b-authentication-authorization--security) and [CI/CD](../../topics/08-cloud-devops-integration/ANSWERS.md#e-cicd--operations).
- [12 Debugging](../../topics/12-debugging-troubleshooting/ANSWERS.md): [production troubleshooting scenarios](../../topics/12-debugging-troubleshooting/ANSWERS.md#b-production-troubleshooting-scenarios).
- [10 Behavioral](../../topics/10-behavioral/ANSWERS.md): prepare stories for ownership, conflict, failure and technical judgement.
- [14 Questions to ask](../../topics/14-questions-to-ask/ANSWERS.md): [to an engineer interviewer](../../topics/14-questions-to-ask/ANSWERS.md#to-an-engineer-interviewer) and [to the hiring manager](../../topics/14-questions-to-ask/ANSWERS.md#to-the-hiring-manager).

## Question lists and cheatsheets

Use the [SDE row of the skills table](../../skills/README.md#what-to-prepare-by-level) to choose an order, then add the tools named in the job description.

| Area | Question lists | Cheatsheets |
|---|---|---|
| Design and code quality | [OOP, design patterns & LLD](../../skills/oop-design-patterns-and-lld.md), [Testing & quality](../../skills/testing-and-quality.md), [Git & code review](../../skills/git-and-code-review.md) | [OOP & design patterns](../../cheatsheets/oop-and-design-patterns.md), [Testing](../../cheatsheets/testing.md), [Linux & Git commands](../../cheatsheets/linux-and-git-commands.md) |
| Algorithms and fundamentals | [Linux & networking](../../skills/linux-and-networking.md) | [Data structures & algorithms](../../cheatsheets/data-structures-and-algorithms.md), [CS fundamentals](../../cheatsheets/cs-fundamentals.md), [Networking & web](../../cheatsheets/networking-and-web.md) |
| Your language | [Java & Spring Boot](../../skills/java-and-spring-boot.md), [Python](../../skills/python.md), [Go](../../skills/go.md), [Node.js & TypeScript](../../skills/nodejs-and-typescript.md), [.NET & C#](../../skills/dotnet-and-csharp.md), [PHP & Laravel](../../skills/php-and-laravel.md), [Django, FastAPI & Flask](../../skills/django-fastapi-and-flask.md) | [Programming languages](../../cheatsheets/programming-languages.md), [Backend frameworks](../../cheatsheets/backend-frameworks.md) |
| APIs and design | [API design & microservices](../../skills/api-design-and-microservices.md), [gRPC, GraphQL & protobuf](../../skills/grpc-graphql-and-protobuf.md), [System design](../../skills/system-design.md), [Authentication & authorisation](../../skills/authentication-and-authorization.md) | [API design](../../cheatsheets/api-design.md), [System design](../../cheatsheets/system-design.md), [Distributed systems](../../cheatsheets/distributed-systems.md), [Security & auth](../../cheatsheets/security-and-auth.md) |
| Data and messaging | [PostgreSQL & data modelling](../../skills/postgresql-and-data-modelling.md), [MySQL](../../skills/mysql.md), [Redis & caching](../../skills/redis-and-caching.md), [Kafka & event streaming](../../skills/kafka-and-event-streaming.md), [RabbitMQ, SQS & task queues](../../skills/rabbitmq-sqs-and-task-queues.md) | [Databases & SQL](../../cheatsheets/databases-and-sql.md), [Search & caching](../../cheatsheets/search-and-caching.md), [Messaging & streaming](../../cheatsheets/messaging-and-streaming.md) |
| Shipping and running | [Docker & CI/CD](../../skills/docker-and-ci-cd.md), [AWS](../../skills/aws.md), [Observability & reliability](../../skills/observability-and-reliability.md), [AI-assisted engineering](../../skills/ai-assisted-engineering.md) | [CI/CD & infrastructure as code](../../cheatsheets/ci-cd-and-iac.md), [AWS](../../cheatsheets/aws.md), [Observability & reliability](../../cheatsheets/observability-and-reliability.md) |
| Interview method | [Technical leadership & behavioral](../../skills/technical-leadership-and-behavioral.md) (for senior levels) | [Interview frameworks](../../cheatsheets/interview-frameworks.md) |

## Role-specific questions

### Role and levels
- [ ] What changes in expectations between a junior, mid-level and senior backend engineer?
- [ ] Walk me through a backend system you built or changed. What would you do differently?
- [ ] How do you decide that a piece of work is ready to ship?

### Coding round
- [ ] How do you approach a 45-minute coding interview?
- [ ] Implement an LRU cache and make it safe to use from several threads.
- [ ] Implement a token-bucket rate limiter.
- [ ] Write a retry helper with exponential backoff and jitter. When should a call not be retried?

### Low-level design
- [ ] How do you approach a low-level design or machine coding round?
- [ ] Design the classes for a discount engine that must support new rule types over time.
- [ ] When would you avoid a design pattern, and when do you prefer composition to inheritance?

### API and service design
- [ ] Design a REST API for creating and retrieving orders.
- [ ] How do you make a POST endpoint idempotent?
- [ ] Compare offset and cursor pagination.
- [ ] How do you change an API without breaking existing clients?
- [ ] When would you use REST, gRPC or GraphQL?

### Databases
- [ ] A query that used to be fast is now slow. How do you diagnose and fix it?
- [ ] How do you choose indexes, and what is the cost of adding one?
- [ ] Explain the transaction isolation levels and the anomalies each one allows.
- [ ] Two customers try to buy the last item at the same moment. How do you prevent overselling?
- [ ] How do you rename a column or change a schema on a live table without downtime?
- [ ] What is the N+1 query problem, and how do you find and fix it?
- [ ] When would you choose a relational database over a NoSQL store?

### Caching and messaging
- [ ] Explain cache-aside. How do you handle invalidation and a cache stampede?
- [ ] How do you update a database and publish an event without losing one of them?
- [ ] What does at-least-once delivery mean for a consumer, and how do you handle poison messages?
- [ ] Compare Kafka, RabbitMQ and SQS for backend work.

### Concurrency and runtime
- [ ] Compare processes, threads and async I/O. Which suits which workload?
- [ ] What causes a deadlock, and how do you prevent one?
- [ ] Requests are timing out with "connection pool exhausted". What do you check?

### System design for backend engineers
- [ ] How do you run a system design round at SDE level, and how do you do a quick capacity estimate?
- [ ] Design a webhook delivery service.
- [ ] Design a backend that accepts orders even when the payment provider is down.

### Testing, security and production
- [ ] What does a good test strategy look like for a backend service, and what do you mock?
- [ ] p99 latency doubled after a deployment. Walk through your investigation.
- [ ] What telemetry do you add to a new service before launch?
- [ ] How do you roll out a risky change safely?
- [ ] Explain authentication and authorisation, sessions and JWTs, and how you prevent common API vulnerabilities.

### Engineering practice
- [ ] What do you look for in a code review?
- [ ] How do you use AI coding assistants without lowering quality?

### Behavioral
- [ ] Tell me about a production bug you caused. What happened next?
- [ ] Tell me about a time you disagreed with a senior engineer about a technical decision.
- [ ] Tell me about a time you had to balance a deadline against code quality.
- [ ] Tell me about something difficult you had to learn quickly.

## Model answers

### Role and levels

#### What changes in expectations between a junior, mid-level and senior backend engineer?

Levels differ mainly in scope, ambiguity and how much other people benefit from your work. Exact definitions vary, so read the company's ladder if it is public.

| | Junior | Mid-level | Senior |
|---|---|---|---|
| Scope | A task or small feature | A feature or small service | A system, or a project across a team |
| Ambiguity | Problem and approach are given | Problem is given, approach is yours | You help define the problem |
| Design | Follows existing patterns | Designs components, asks for review | Leads designs, weighs long-term cost |
| Production | Fixes bugs with support | Joins on-call, resolves most incidents | Improves reliability and leads hard incidents |
| Others | Learns from reviews | Reviews code, helps juniors | Mentors, sets standards, influences decisions |

In the interview, show you already work at the target level: describe scope you owned, decisions you made and problems you found without being told.

#### Walk me through a backend system you built or changed. What would you do differently?

Prepare one project you can discuss for 20 minutes. A useful order:

1. **Problem and scale (1 minute):** what it does, for whom, with rough numbers such as requests per second, data size and team size.
2. **Architecture (3 minutes):** one diagram following a request from start to finish.
3. **Your part:** be precise about what you did, as distinct from what the team did.
4. **Two or three decisions:** the alternatives, why you chose as you did and what you gave up.
5. **A hard moment:** an incident, a wrong assumption or a performance problem.
6. **Results:** latency, cost, error rate or delivery speed, with numbers where you can share them.
7. **What you would change:** a real answer, which shows judgement and honesty.

Expect interviewers to pick one component and go deeper until you reach the limit of your knowledge. Check company confidentiality rules beforehand, and generalise names and numbers where needed.

#### How do you decide that a piece of work is ready to ship?

- **It does what was asked,** including the edge cases agreed with product.
- **Tests cover the behaviour and the failure paths,** and CI is green.
- **It has been reviewed** by someone who understands the area.
- **It is observable:** logs, metrics and alerts show whether it works.
- **It can be undone:** a feature flag, a rollback, or a migration that stays backwards compatible.
- **Risk matches the rollout:** high-risk changes get a canary and a person watching.
- **The people affected know:** on-call, support and any client teams.

Ready does not mean perfect. Record what you left out and why, as tickets rather than memories.

### Coding round

#### How do you approach a 45-minute coding interview?

1. **Clarify (3–5 minutes):** input size and types, duplicates, empty input, what to return when there is no answer. Work one example by hand.
2. **Give the simple solution first,** with its complexity, then improve it. Name the pattern, such as a sliding window, a hash map or BFS.
3. **Agree before coding:** "That is O(n) time and O(k) space. Shall I implement it?"
4. **Code while narrating,** using clear names and small helpers.
5. **Test by hand:** trace the example, then try edge cases, and fix bugs calmly.
6. **Finish with complexity and follow-ups:** larger input, concurrency, or running across several servers.

If you get stuck, say what you have tried and which constraint is causing the trouble. Interviewers can only give credit for reasoning they can hear. More detail: [D. Questions asked during/after coding](../../topics/02-coding/ANSWERS.md#d-questions-asked-duringafter-coding).

#### Implement an LRU cache and make it safe to use from several threads.

A hash map gives O(1) lookup, and a doubly linked list gives O(1) recency updates. Python's `OrderedDict` combines both.

```python
from collections import OrderedDict
from threading import Lock


class LRUCache:
    def __init__(self, capacity: int):
        if capacity <= 0:
            raise ValueError("capacity must be positive")
        self._capacity = capacity
        self._items = OrderedDict()
        self._lock = Lock()

    def get(self, key, default=None):
        with self._lock:
            if key not in self._items:
                return default
            self._items.move_to_end(key)
            return self._items[key]

    def put(self, key, value):
        with self._lock:
            if key in self._items:
                self._items.move_to_end(key)
            self._items[key] = value
            if len(self._items) > self._capacity:
                self._items.popitem(last=False)
```

Points to mention:
- Even `get` changes the order, so it needs the same lock as `put`. A read-write lock does not help here.
- One lock is simple but limits throughput. Sharding by key hash into several caches reduces contention.
- If asked to avoid `OrderedDict`, build the linked list yourself with dummy head and tail nodes.
- Follow-ups: time-to-live, size in bytes rather than item count, and hit-rate metrics.

#### Implement a token-bucket rate limiter.

A bucket holds up to `capacity` tokens and refills at a steady rate. Each request takes a token, so short bursts are allowed while the long-run rate stays limited.

```python
import threading
import time


class TokenBucket:
    def __init__(self, rate_per_second: float, capacity: float, clock=time.monotonic):
        self._rate = rate_per_second
        self._capacity = capacity
        self._clock = clock
        self._tokens = capacity
        self._updated = clock()
        self._lock = threading.Lock()

    def allow(self, cost: float = 1) -> bool:
        with self._lock:
            now = self._clock()
            elapsed = now - self._updated
            self._tokens = min(self._capacity, self._tokens + elapsed * self._rate)
            self._updated = now
            if self._tokens >= cost:
                self._tokens -= cost
                return True
            return False
```

Points to mention:
- Refilling on demand needs no background thread, so idle keys cost nothing.
- A monotonic clock is not affected by system clock changes, and an injected clock makes tests fast.
- Keep one bucket per client key, and evict idle ones.
- Across several servers, move the state to Redis and update it atomically with a Lua script. See [Rate limiter (distributed)](../../topics/03-system-design/ANSWERS.md#rate-limiter-distributed).
- Return `429 Too Many Requests` with a `Retry-After` header.

#### Write a retry helper with exponential backoff and jitter. When should a call not be retried?

```python
import random
import time


def retry(fn, attempts=5, base=0.1, cap=5.0,
          retryable=(TimeoutError, ConnectionError), sleep=time.sleep):
    for attempt in range(attempts):
        try:
            return fn()
        except retryable:
            if attempt == attempts - 1:
                raise
            sleep(random.uniform(0, min(cap, base * 2 ** attempt)))
```

Jitter stops many clients from retrying at the same moment. This version uses "full jitter", which picks a random wait up to the backoff limit.

Do not retry when:
- **The error is permanent:** validation failures, `401`, `403` and `404`.
- **The operation is not idempotent** and there is no idempotency key, because a timeout does not prove the first attempt failed.
- **The overall deadline has passed:** the caller has already given up.
- **The dependency is overloaded:** use a retry budget or circuit breaker so retries do not make the outage worse.

Avoid retrying at every layer. Three retries at each of three layers can turn one request into 27 calls.

### Low-level design

#### How do you approach a low-level design or machine coding round?

1. **Clarify scope (5 minutes):** list the required features and agree what is out of scope.
2. **Find the entities and responsibilities:** nouns become candidate classes, and each class should have one reason to change.
3. **Define interfaces first,** especially at points likely to change, such as pricing rules or storage.
4. **Implement the core flow end to end** before adding extras. Working code beats a complete design that does not run.
5. **Use in-memory storage behind a repository interface,** unless the problem requires otherwise.
6. **Handle errors and concurrency where they matter,** such as two users booking the same seat.
7. **Add a few tests or a small demo,** then explain how a new requirement would fit.

Common mistakes: naming patterns without needing them, spending 30 minutes on diagrams, and putting all logic in one "manager" class. Worked examples: [02-coding section C](../../topics/02-coding/ANSWERS.md#c-object-oriented--low-level-design-code-it).

#### Design the classes for a discount engine that must support new rule types over time.

Clarify: can discounts stack, what order do they apply in, is there a maximum, and how are amounts rounded?

```python
from dataclasses import dataclass
from decimal import Decimal
from typing import Protocol


@dataclass(frozen=True)
class Cart:
    subtotal: Decimal
    item_count: int
    customer_tier: str


class DiscountRule(Protocol):
    def amount_off(self, cart: Cart) -> Decimal: ...


@dataclass(frozen=True)
class PercentOff:
    percent: Decimal

    def amount_off(self, cart: Cart) -> Decimal:
        return cart.subtotal * self.percent / 100


@dataclass(frozen=True)
class TierOnly:
    tier: str
    rule: DiscountRule

    def amount_off(self, cart: Cart) -> Decimal:
        return self.rule.amount_off(cart) if cart.customer_tier == self.tier else Decimal(0)


class BestOfPolicy:
    def __init__(self, rules: list[DiscountRule]):
        self._rules = rules

    def total_off(self, cart: Cart) -> Decimal:
        best = max((rule.amount_off(cart) for rule in self._rules), default=Decimal(0))
        return min(best, cart.subtotal)
```

Why this shape:
- **Strategy:** each rule is a small class, so a new rule type adds a class without changing the others.
- **Decorator or composition:** `TierOnly` wraps any rule, so conditions and amounts combine freely.
- **A separate policy object:** whether rules stack or the best one wins is a business decision that can change on its own.
- **`Decimal` for money:** binary floats cannot represent most decimal amounts exactly.

Follow-ups: load rules from a database, record which rule applied for audit, and add start and end dates.

#### When would you avoid a design pattern, and when do you prefer composition to inheritance?

Use a pattern when it solves a problem you have now or can clearly expect. Avoid it when:
- There is only one implementation and no sign of another; a plain function is enough.
- It adds indirection that makes the code harder to follow.
- The language already provides it, for example passing a function rather than writing a strategy class.

Prefer **composition** by default: behaviour can be combined and swapped at runtime, tests can substitute parts, and changes to one part affect others less. **Inheritance** fits a real "is a" relationship where every subclass can stand in for its parent (the Liskov substitution principle), or where a framework requires it. Warning signs are deep hierarchies, subclasses that disable parent methods, and classes such as `PremiumDiscountedExpressOrder` that multiply as features combine.

### API and service design

#### Design a REST API for creating and retrieving orders.

```text
POST   /v1/orders                 Idempotency-Key: <uuid>
GET    /v1/orders/{order_id}
GET    /v1/orders?customer_id=c1&status=PAID&limit=50&cursor=abc
POST   /v1/orders/{order_id}/cancel
```

A request and an error response might look like this:

```json
{ "customer_id": "c1", "currency": "GBP",
  "items": [{ "sku": "A1", "quantity": 2 }] }
```

```json
{ "error": { "code": "INSUFFICIENT_STOCK", "message": "SKU A1 has 1 unit left.",
             "request_id": "req_123" } }
```

Design points:
- **Status codes:** `201` with a `Location` header on creation; `202` if processing continues in the background; `400` for malformed input; `401` and `403` for authentication and permission failures; `404`; `409` for a state conflict; `422` for valid syntax that breaks a business rule; `429` for rate limits; `5xx` for server faults.
- **Actions:** a transition such as cancel can be a sub-resource, because it is not a simple field update and has its own rules.
- **Money:** integer minor units (such as pence) or decimal strings, always with a currency.
- **Identifiers:** random IDs, so that competitors cannot count orders and users cannot guess other users' IDs. Still check ownership on every request.
- **Time:** ISO 8601 timestamps in UTC.
- **Evolution:** add fields freely, and tell clients to ignore fields they do not recognise.

#### How do you make a POST endpoint idempotent?

The client sends a unique `Idempotency-Key` per logical operation and reuses it on retries.

1. In one transaction, insert a row with `(client_id, key, request_hash, status)`, relying on a unique constraint.
2. If the key is new, do the work and store the response with the key, in the same transaction where possible.
3. If the key exists and is complete, return the stored response.
4. If the key exists and is still in progress, return `409` or ask the client to retry shortly.
5. If the key exists with a different request hash, return an error, because the client has misused the key.

Keep keys for a defined period, such as 24 hours. If the work calls an external system, pass an idempotency key to that system too, because a database transaction cannot undo an external charge. See also the [Payment system / ledger](../../topics/03-system-design/ANSWERS.md#payment-system--ledger) prompt.

#### Compare offset and cursor pagination.

| | Offset (`?offset=1000&limit=50`) | Cursor (`?after=<token>&limit=50`) |
|---|---|---|
| Query | `ORDER BY id LIMIT 50 OFFSET 1000` | `WHERE (created_at, id) < (?, ?) ORDER BY created_at DESC, id DESC LIMIT 50` |
| Deep pages | Slow, because the database still reads the skipped rows | Stays fast with a matching index |
| Changing data | Items can repeat or be skipped when rows are inserted or deleted | Stable position |
| Jump to page 40 | Easy | Not possible |

Use cursors for feeds, large tables and public APIs. Offset is acceptable for small admin screens that need page numbers. The sort must include a unique tie-breaker such as `id`. Make the cursor opaque, for example base64-encoded, so that you can change it later.

#### How do you change an API without breaking existing clients?

- **Additive changes are safe:** new optional fields, new endpoints, and new enum values only if clients were told to expect them.
- **Breaking changes include:** removing or renaming fields, changing types or meanings, making an optional field required, and tightening validation.
- **For a breaking change:**
  1. Add the new version or field alongside the old one.
  2. Measure usage per client and per version.
  3. Announce a deprecation date, and use response headers such as `Deprecation` and `Sunset` where clients will read them.
  4. Help the biggest clients migrate.
  5. Remove the old version once usage reaches zero or the agreed date passes.
- **Versioning styles:** a path prefix (`/v1/`) is visible and simple; a header or date-based version gives more control but takes more tooling.
- **Prevent accidents:** consumer-driven contract tests, such as Pact, and OpenAPI diff checks in CI.

#### When would you use REST, gRPC or GraphQL?

| | REST over JSON | gRPC | GraphQL |
|---|---|---|---|
| Best for | Public APIs and simple service calls | Internal service-to-service calls | Clients with varied data needs, such as mobile and web |
| Contract | OpenAPI (optional) | Protobuf (required) | Schema (required) |
| Strengths | Universal tooling, HTTP caching, easy debugging | Compact payloads, generated clients, streaming, deadlines | Clients fetch exactly what they need from one endpoint |
| Costs | Over-fetching and under-fetching; contract can drift | Harder to use directly from browsers; less readable on the wire | N+1 risks in resolvers, harder caching and rate limiting, query cost controls needed |

A common mix is REST at the public edge and gRPC inside. Do not adopt GraphQL or gRPC for a small system unless a real problem justifies the tooling.

### Databases

#### A query that used to be fast is now slow. How do you diagnose and fix it?

1. **Measure:** find the query with slow query logs or `pg_stat_statements` (PostgreSQL). Check when it changed, and whether a deployment, data growth or a traffic change lines up with that time.
2. **Read the plan:** `EXPLAIN (ANALYZE, BUFFERS)` in PostgreSQL, with care on writes because `ANALYZE` executes the query. MySQL has `EXPLAIN ANALYZE` too *(verify)*. Look for sequential scans on large tables, row estimates far from actual counts, and large sorts or hash joins spilling to disk.
3. **Common causes and fixes:**
   - **Missing or unusable index:** add one, or stop wrapping the indexed column in a function.
   - **Stale statistics:** run `ANALYZE`, because estimates drive the plan.
   - **Data growth or skew:** one large tenant or a changed value distribution can make an old plan a bad one.
   - **Lock waits:** the query may be blocked rather than slow, so check `pg_locks` and `pg_stat_activity`.
   - **Table bloat:** check vacuum health in PostgreSQL.
   - **Fetching too much:** select fewer columns, paginate, or remove an unnecessary join.
4. **Fix and verify:** compare the plan and latency before and after, and watch write cost if you added an index. Create it with `CREATE INDEX CONCURRENTLY` on a live PostgreSQL table.

See also [Database queries suddenly became 10x slower](../../topics/12-debugging-troubleshooting/ANSWERS.md#database-queries-suddenly-became-10x-slower).

#### How do you choose indexes, and what is the cost of adding one?

Start from real queries, not from columns.

- **Composite column order:** equality columns first, then the range or sort column. An index on `(tenant_id, status, created_at)` supports `WHERE tenant_id = ? AND status = ? ORDER BY created_at DESC`.
- **Leftmost prefix:** that index helps queries on `tenant_id` alone, but usually not on `status` alone.
- **Selectivity:** an index on a column where most rows share the same value rarely helps. A partial index, such as `WHERE status = 'PENDING'`, can be small and effective.
- **Covering indexes:** including all needed columns lets the database answer from the index alone.
- **Expressions:** `WHERE lower(email) = ?` needs an index on `lower(email)`.

Costs: slower writes, because every index must be updated; extra storage and memory; and more work for vacuum or page maintenance. Review unused indexes regularly, and remember that a replica may use indexes the primary does not.

#### Explain the transaction isolation levels and the anomalies each one allows.

| Level | Dirty read | Non-repeatable read | Phantom read |
|---|---|---|---|
| `READ UNCOMMITTED` | Possible | Possible | Possible |
| `READ COMMITTED` | No | Possible | Possible |
| `REPEATABLE READ` | No | No | Possible by the SQL standard |
| `SERIALIZABLE` | No | No | No |

- **Dirty read:** seeing another transaction's uncommitted change.
- **Non-repeatable read:** reading the same row twice and getting different values.
- **Phantom read:** rerunning a range query and getting different rows.
- **Lost update and write skew:** two transactions read the same data and write based on it. These are the bugs that most often reach production.

Real databases differ from the standard. PostgreSQL defaults to `READ COMMITTED` and its `REPEATABLE READ` uses snapshot isolation, which prevents phantoms but still allows write skew. MySQL InnoDB defaults to `REPEATABLE READ` *(verify)*. With `SERIALIZABLE`, the application must be ready to retry transactions that the database aborts.

#### Two customers try to buy the last item at the same moment. How do you prevent overselling?

Do the check and the update in one atomic step:

```sql
UPDATE inventory
SET available = available - 1
WHERE sku = 'A1' AND available >= 1;
-- 0 rows updated means the item is out of stock
```

Other options:
- **Pessimistic lock:** `SELECT ... FOR UPDATE` holds the row until commit. It is simple and correct, but limits throughput when one row is busy. Keep transactions short, and never hold a lock during a call to an external service.
- **Optimistic lock:** add a `version` column and use `WHERE version = ?`. It suits low contention, and the application retries on conflict.
- **Reservations:** hold stock with an expiry time while payment runs, and release it if payment fails. This is the usual approach for checkout, because payment is slow and can fail.
- **Very busy items:** split stock into several counters, or put purchases through a queue.

A check in the application followed by a separate update is the classic race condition. A `CHECK (available >= 0)` constraint is a useful safety net. See also the [ticket booking](../../topics/03-system-design/ANSWERS.md#ticket-booking-ticketmaster-with-concurrency-on-seats) prompt.

#### How do you rename a column or change a schema on a live table without downtime?

During a rolling deployment, old and new code run together, so every step must work with both. Use **expand and contract**:

1. **Expand:** add the new column as nullable.
2. **Dual write:** deploy code that writes both columns and still reads the old one.
3. **Backfill:** copy old rows in small batches with pauses, and watch replication lag.
4. **Switch reads:** deploy code that reads the new column, ideally behind a flag. Compare results first if the data is important.
5. **Stop writing the old column,** after confirming nothing depends on it.
6. **Contract:** drop the old column in a later release.

Watch for locks too. Some operations rewrite a table or wait on a strong lock that blocks all other queries behind it. In PostgreSQL, use `CREATE INDEX CONCURRENTLY`, add constraints as `NOT VALID` and validate them separately, and set a `lock_timeout` on migrations *(verify the behaviour for your version)*. For MySQL, tools such as gh-ost or pt-online-schema-change handle large table changes.

#### What is the N+1 query problem, and how do you find and fix it?

The code runs one query to load a list, then one more query per item. ORMs make this easy to do by accident through lazy loading.

```python
orders = Order.objects.filter(customer_id=customer_id)   # 1 query
for order in orders:
    print(order.customer.name)                           # N more queries
```

**Find it:** query counts in traces or logs, an ORM debug tool, or a test that fails when a request's query count grows with the data.

**Fix it:**
- Eager load with a join or a second batched query (`select_related` and `prefetch_related` in Django, `JOIN FETCH` in JPA, `with()` in Laravel).
- Use a batched `WHERE id IN (...)` query, or a DataLoader in GraphQL resolvers.
- Do not over-correct: joining many large one-to-many relations can multiply rows and be slower than two simple queries.

#### When would you choose a relational database over a NoSQL store?

Start with a relational database unless you have a specific reason not to. It offers transactions, constraints, flexible queries and mature tools. Modern relational databases can handle far more load than many teams expect.

| Consider | When |
|---|---|
| Key-value or wide-column (DynamoDB, Cassandra) | Very high write volume, known access patterns, simple partition-key lookups, and easy horizontal scale |
| Document (MongoDB) | Data is naturally a self-contained document with a varying shape, usually read and written as a whole |
| Search (Elasticsearch, OpenSearch) | Full-text search and relevance, usually as a secondary copy of the data |
| Time-series or columnar (ClickHouse) | Large analytical scans and aggregates, rather than transactions |
| In-memory (Redis) | Caching, counters, short-lived state |

Costs of NoSQL include designing around access patterns up front, doing joins in the application, and weaker guarantees around transactions, depending on the product *(verify)*. Using several stores is common: for example, a relational system of record with a search index built from it.

### Caching and messaging

#### Explain cache-aside. How do you handle invalidation and a cache stampede?

**Cache-aside:** on a read, check the cache; on a miss, read the database and fill the cache with a time-to-live (TTL). On a write, update the database and then delete the cache key.

Delete rather than update the key, because two concurrent writers can leave a stale value if they update the cache in a different order from the database. A race can still leave a stale value, so a TTL remains the safety net. If staleness really matters, version the keys or use change data capture to invalidate.

**Stampede:** a popular key expires and thousands of requests hit the database together. Defences:
- **Single flight:** one request recomputes while others wait or receive the old value.
- **TTL jitter:** add randomness so that keys do not all expire at once.
- **Early refresh:** recompute shortly before expiry, in the background.
- **Serve stale:** return the old value while refreshing.
- **Protect the database:** a rate limit or circuit breaker on the miss path.

Also consider caching "not found" results for a short time, and alerting on hit rate. Plan for a cold cache, because the database must survive a restart of the cache.

#### How do you update a database and publish an event without losing one of them?

Writing to the database and then to the broker is a **dual write**. A crash between the two loses the event, and reversing the order can publish an event for a change that never happened.

Use the **transactional outbox**:

```sql
BEGIN;
UPDATE orders SET status = 'PAID' WHERE id = 42;
INSERT INTO outbox (id, aggregate_id, type, payload)
VALUES ('e1', 42, 'OrderPaid', '{"order_id": 42}');
COMMIT;
```

A separate relay publishes outbox rows to the broker, either by polling or by reading the database log with change data capture (for example Debezium), and marks them as sent.

Consequences:
- Delivery is **at least once,** because the relay may crash after publishing and before marking the row. Consumers must remove duplicates using the event ID.
- Keep events for one entity in order by using its ID as the partition key.
- Monitor the age of the oldest unsent row, and clean up old rows.

#### What does at-least-once delivery mean for a consumer, and how do you handle poison messages?

A message may arrive more than once, for example when a consumer crashes after doing the work but before acknowledging. Acknowledge only after the work is done, and make processing idempotent:

- Store processed message IDs in the same transaction as the business change, with a unique constraint.
- Or make the operation naturally idempotent, for example by setting a status rather than adding to a balance.

Exactly-once is usually at-least-once delivery plus idempotent processing. Kafka's transactions cover read-process-write within Kafka, but not calls to external systems.

A **poison message** always fails, and can block a queue or partition:
1. Retry a limited number of times, with backoff.
2. Move it to a **dead-letter queue** with the error and metadata.
3. Alert on queue depth and message age.
4. Provide a safe way to inspect, fix and replay.

Separate permanent errors (bad data, go straight to the dead-letter queue) from temporary errors (a dependency is down, so slow down rather than dead-lettering everything).

#### Compare Kafka, RabbitMQ and SQS for backend work.

| | Kafka | RabbitMQ | SQS |
|---|---|---|---|
| Model | Replicated, partitioned log | Broker with exchanges, queues and routing | Managed queue |
| After consumption | Retained for a set period; consumers track an offset | Removed once acknowledged | Removed once deleted |
| Replay | Yes | Limited (streams can replay) *(verify)* | No |
| Ordering | Per partition | Per queue, with caveats on redelivery | FIFO queues only, per message group |
| Scaling consumers | Up to the partition count per group | Competing consumers | Very easy |
| Best for | Event streams, several independent consumers, high throughput | Task queues and flexible routing | Simple AWS task queues with no servers to run |
| Operational cost | Highest, unless managed | Medium | Lowest |

Choose by need. A background job queue rarely needs Kafka, while several teams consuming and replaying the same events usually do not fit SQS. More detail: [Messaging & streaming](../../cheatsheets/messaging-and-streaming.md) and [RabbitMQ](../../cheatsheets/rabbitmq.md).

### Concurrency and runtime

#### Compare processes, threads and async I/O. Which suits which workload?

| | Processes | Threads | Async I/O (event loop) |
|---|---|---|---|
| Memory | Separate | Shared within a process | Shared, usually one thread per loop |
| Isolation | Strong | A crash or memory bug affects the whole process | A blocking call stalls everything on the loop |
| Switching cost | Highest | Medium | Lowest |
| Best for | CPU-heavy work, or strong isolation | Mixed work, in languages with true parallelism | Many concurrent I/O waits |

Runtime matters:
- **Python:** the global interpreter lock (GIL) limits CPU parallelism in standard builds, so use processes for CPU work and threads or `asyncio` for I/O. Free-threaded builds are changing this *(verify)*.
- **Node.js:** one event loop for JavaScript, with worker threads for CPU work.
- **Go:** goroutines are multiplexed onto operating system threads by the runtime.
- **Java:** platform threads, plus virtual threads since Java 21 *(verify)*.

Rule of thumb: a web service mostly waits on I/O, so it benefits from async or many light threads. Size CPU-bound pools near the number of cores.

#### What causes a deadlock, and how do you prevent one?

A deadlock needs four conditions together: mutual exclusion, hold and wait, no pre-emption and a circular wait. Remove any one of them.

Typical example: transaction A locks row 1 and waits for row 2, while transaction B locks row 2 and waits for row 1.

Prevention:
- **Consistent lock order:** for example, always lock accounts in ascending ID order during a transfer.
- **Short transactions:** no network calls while holding locks.
- **Timeouts:** `lock_timeout` in the database and `tryLock` with a timeout in code.
- **Fewer locks:** atomic updates, optimistic concurrency or immutable data.

Databases detect deadlocks and abort one transaction, so the application must retry it. To diagnose, read the database's deadlock log, or take a thread dump (`jstack` for Java, goroutine dump for Go) to see who holds what.

#### Requests are timing out with "connection pool exhausted". What do you check?

The pool is usually a symptom. Connections are being held too long or demand has exceeded capacity.

1. **Pool metrics:** active, idle and waiting connections, and wait time. Did this start after a deployment or a traffic spike?
2. **Database:** slow queries and lock waits in `pg_stat_activity` or `SHOW PROCESSLIST`. One slow query can hold many connections.
3. **Long transactions:** code that makes an HTTP call while a transaction is open is the classic cause.
4. **Leaks:** connections not returned on an error path. Use `with` or `try-with-resources`, and turn on the pool's leak detection.
5. **Sizing:** total connections equal instances × pool size. Autoscaling can exceed the database's `max_connections`, and PgBouncer or RDS Proxy can help.
6. **Retries:** a retry storm can make the problem worse.

Do not begin by increasing the pool size, because more concurrent queries can overload the database further. Mitigate now by rolling back or killing the bad query, then fix the cause. Set timeouts for acquiring a connection and for statements so that failures are quick.

### System design for backend engineers

#### How do you run a system design round at SDE level, and how do you do a quick capacity estimate?

Use a simple sequence, and keep moving:

1. **Requirements (5 minutes):** core features, scale, latency and consistency needs.
2. **Estimate (2 minutes):** only what affects the design.
3. **API and data model (10 minutes):** this is where backend engineers can stand out.
4. **High-level design (10 minutes):** follow one write and one read through the system.
5. **Deep dive (15 minutes):** the hardest part, such as concurrency, failure handling or a growing table.
6. **Wrap-up (3 minutes):** monitoring, risks and what you would build first.

**Estimate example:** 2 million orders a day.
- Average writes: 2,000,000 / 86,400 ≈ 23 per second. At a 10× peak, about 230 per second, which one well-sized relational database can usually handle.
- Rows per year: about 730 million.
- Storage at about 1 KB per order, including indexes: about 730 GB a year, or about 2.2 TB after three years, so plan for partitioning and archiving.

State what the numbers mean for the design. "230 writes a second does not need sharding yet" is more useful than the arithmetic alone. More prompts: [classic system design](../../topics/03-system-design/ANSWERS.md#c-classic-system-design-still-asked).

#### Design a webhook delivery service.

Clarify: event volume, number of subscriber endpoints, the delivery guarantee, ordering needs and retry period.

Design:
- **Intake:** internal services publish events through a transactional outbox to a log or queue.
- **Fan-out:** a dispatcher finds the matching subscriptions, then creates one delivery record per endpoint with its status, attempt count and next attempt time.
- **Delivery workers:** make the HTTP call with short connect and read timeouts, treating any `2xx` as success.
- **Retries:** exponential backoff with jitter for up to about 24–72 hours, then mark as failed and alert the subscriber. Retries are at-least-once, so include a stable event ID for consumers to remove duplicates.
- **Isolation:** one slow endpoint must not delay everyone. Use per-endpoint concurrency limits and queues, plus a circuit breaker that pauses failing endpoints.
- **Security:**
  - Sign payloads with HMAC over the timestamp and body, and support secret rotation.
  - Prevent server-side request forgery: require HTTPS, and block private and internal IP ranges, checking again after DNS resolution and on redirects.
- **Ordering:** do not promise it by default. Include a sequence number or tell consumers to fetch the latest state.
- **Self-service:** delivery logs, manual redelivery and a test event.
- **Observability:** success rate, delivery delay and queue age, by endpoint.

Trade-off to state: per-endpoint queues give the best isolation, but cost more to run than a shared queue with concurrency limits. Many systems start with the shared queue.

#### Design a backend that accepts orders even when the payment provider is down.

Clarify: can the business ship before payment, how long can payment wait, and is a card authorised immediately or charged later?

Design:
1. `POST /orders` validates the request, reserves stock and saves the order as `PAYMENT_PENDING` with an outbox event in one transaction. It returns `202`, with an order ID to poll.
2. A payment worker calls the provider with a stable idempotency key, so a retry cannot charge twice.
3. **Timeouts and a circuit breaker** stop workers from waiting on a provider that is down. Messages wait in the queue, with backoff.
4. **Unknown results:** after a timeout, the payment may or may not have succeeded. Query the provider before retrying, or reconcile against its reports.
5. **State machine:** `PAYMENT_PENDING → PAID → FULFILLING`, or `PAYMENT_FAILED → CANCELLED`. A failure or an expired reservation triggers compensation: release the stock and notify the customer. This is a saga.
6. **Fallback:** a second provider, or a limit on pending value so that business risk stays bounded.
7. **Monitoring:** age of the oldest pending payment, breaker state and payment success rate, with an alert for humans when the delay exceeds what the business accepts.

Mention the business trade-off: accepting orders without payment protects revenue, but risks failed payments and a worse customer experience later. That decision belongs to product and finance. See also [Payment system / ledger](../../topics/03-system-design/ANSWERS.md#payment-system--ledger).

### Testing, security and production

#### What does a good test strategy look like for a backend service, and what do you mock?

- **Unit tests (many):** business rules and edge cases, running in milliseconds with no I/O.
- **Integration tests (some):** real database queries, migrations and broker behaviour. Testcontainers makes this practical, and catches problems such as wrong SQL that a mock would hide.
- **Contract tests:** check agreements between services, using tools such as Pact or schema checks.
- **End-to-end tests (few):** critical user journeys only, because they are slow and flaky.
- **Non-functional tests:** load tests before big launches, and failure injection for timeouts.

**Mocking:** mock things you do not own and that are slow or unreliable, such as third-party APIs, and fake the clock. Avoid mocking your own database layer when the query itself is what matters, and avoid asserting on internal calls. Such tests pass while the behaviour is wrong, and they break on harmless refactors.

**Good tests** fail for one clear reason, run reliably and describe behaviour. Coverage shows what is untested, but does not show that the tests are any good. Quarantine and fix flaky tests quickly, because people stop trusting a suite that fails at random.

#### p99 latency doubled after a deployment. Walk through your investigation.

1. **Protect users first:** if the impact is serious and the timing matches the deployment, roll back. Investigate afterwards.
2. **Scope it:** which endpoints, instances, regions or tenants? Is p50 affected too? A p99-only change often points to contention, garbage collection, retries or a rare slow path.
3. **Compare:** read the deployment diff, and compare the new version with the old if a canary still exists. Check flags and configuration too.
4. **Follow traces:** take slow requests and find the span that grew: database, cache, downstream call or the service itself.
5. **Check saturation:** CPU, memory, garbage collection pauses, thread and connection pools, queue depth.
6. **Check the usual suspects:**
   - A new N+1 query or a missing index.
   - A lower cache hit rate, such as a changed key format or a cold cache.
   - A new synchronous call or a larger payload.
   - More retries or timeouts on a dependency.
7. **Confirm:** reproduce the problem or test a fix, and verify the metric returns to normal.
8. **Prevent it:** a canary check on latency, an SLO alert or a load test.

#### What telemetry do you add to a new service before launch?

- **Metrics:** the RED set for requests (rate, errors, duration by route) and the USE set for resources (utilisation, saturation, errors). Add business metrics such as orders created, and queue age for consumers. Keep label values low in cardinality: no user IDs in metric labels.
- **Logs:** structured JSON with a request ID and trace ID. Log at clear levels, never log secrets or personal data, and sample noisy success logs.
- **Traces:** OpenTelemetry instrumentation across services, with context passed through queues too.
- **Health endpoints:** a liveness check that stays simple, and a readiness check that reflects whether the instance can serve traffic.
- **Alerts:** alert on user-visible symptoms using SLOs, such as error rate and latency burn, rather than on every CPU spike. Each alert links to a runbook.
- **Dashboard:** one page that shows whether the service is healthy, with deployments marked on it.

The test: at 3 a.m., could someone unfamiliar with this service tell what is broken and who is affected?

#### How do you roll out a risky change safely?

- **Separate deployment from release:** ship code turned off behind a feature flag, then enable it gradually.
- **Canary:** send 1%, then 10%, then 50% of traffic, comparing errors and latency with the old version. Automate rollback when possible.
- **Dark launch or shadow traffic:** run the new path on real requests and compare results without affecting users. Take care with side effects.
- **Backwards compatibility:** the old and new versions run side by side, and rollback must still work. Use expand and contract for schema changes.
- **Blast radius:** start with internal users, one region or low-risk tenants.
- **Prepare:** define success metrics and rollback triggers in advance, avoid releasing before a holiday, and make sure someone is watching.
- **Clean up:** remove old flags, or they become hidden complexity.

Database migrations and data backfills often cannot simply be rolled back, so they need their own plan.

#### Explain authentication and authorisation, sessions and JWTs, and how you prevent common API vulnerabilities.

**Authentication** establishes who you are. **Authorisation** decides what you may do, and must be checked on the server for every request.

| | Server-side session | JWT access token |
|---|---|---|
| State | Stored on the server, with an ID in a cookie | Self-contained, signed token |
| Revocation | Immediate: delete the session | Hard before expiry, so use short lifetimes and refresh tokens |
| Lookup | Session store on every request | Local signature check |
| Good for | Web applications | APIs and service-to-service calls |

JWT checks: verify the signature with a fixed algorithm (never trust the token's `alg` header alone), plus expiry, issuer and audience. JWTs are signed, not encrypted, so do not put secrets in them.

Common API risks (see the [OWASP API Security Top 10](https://owasp.org/API-Security/) *(verify current list)*):
- **Broken object-level authorisation:** user A requests user B's order by changing the ID. Check ownership on every object, and prefer scoping queries by tenant or user.
- **Injection:** use parameterised queries, never string concatenation.
- **Mass assignment:** accept only allowed fields rather than binding the whole request body to a model.
- **Abuse:** rate limits, input size limits and pagination caps.
- **Secrets:** use a secret manager, keep secrets out of code and logs, and rotate them.
- **Passwords:** a slow, salted hash such as Argon2 or bcrypt.

### Engineering practice

#### What do you look for in a code review?

In rough priority order:

1. **Correctness:** does it do what the ticket says, including failures, edge cases and concurrency?
2. **Design:** is this the right place and the right level of abstraction, and does it fit the existing code?
3. **Risk:** migrations, backwards compatibility, security, performance and rollback.
4. **Tests:** would they catch a regression, and do they test behaviour?
5. **Readability:** could someone else maintain this in six months?

Leave style to formatters and linters. Comment on the code and not the person, explain why, and separate blocking comments from suggestions ("nit:"). Review quickly, because waiting is expensive for the author. As an author, keep pull requests small, describe why the change exists, and review your own diff first.

#### How do you use AI coding assistants without lowering quality?

- **Use them where checking is easy:** boilerplate, tests, unfamiliar syntax, refactoring, explaining code and first drafts.
- **Stay accountable:** review generated code as carefully as a colleague's, and be able to explain every line you submit.
- **Watch the typical failures:** invented APIs and packages, outdated library usage, missed edge cases, insecure defaults and tests that only confirm what the code already does.
- **Give it context:** existing conventions, relevant files, and precise requirements.
- **Rely on your safety nets:** tests, types, linters, security scanning and code review all still apply.
- **Protect data:** follow company policy on what code and data can be sent to which tools.
- **Keep learning:** use the tool to understand a concept, not just to get past it, and be able to work without it. Most interview rounds still expect you to reason on your own unless told otherwise, and some companies now run rounds that allow AI tools *(verify for each company)*.

### Behavioral

#### Tell me about a production bug you caused. What happened next?

**What they look for:** you own the mistake, respond calmly, fix the cause and improve the system without blaming anyone.

**STAR outline:**
- **S:** [What the change was, and what the impact was on users or data]
- **T:** [Your responsibility during the incident]
- **A:** [How you detected it, how you mitigated it, how you found the root cause, and what you changed afterwards]
- **R:** [Impact contained, with numbers; the prevention that remains in place]

**Example shape (invented, replace it with your own story):** An engineer deploys a schema change that locks a busy table, and checkout errors rise. They flag it in the incident channel, roll back the release and help clear the waiting queries. Errors recover within a few minutes. They then write the review, add a lock timeout to the migration tool and add a CI check for unsafe migrations. The class of incident does not recur.

#### Tell me about a time you disagreed with a senior engineer about a technical decision.

**What they look for:** you raise concerns respectfully, use evidence, listen, and support the outcome even if it does not go your way.

**STAR outline:**
- **S:** [The decision and why it mattered]
- **T:** [Your view and what was at risk]
- **A:** [How you tested your view: a prototype, data or a written comparison; how you discussed it; how the decision was made]
- **R:** [The outcome, what you learned and how the working relationship went]

**Example shape (invented, replace it with your own story):** A senior engineer proposes adding Kafka for a simple job queue. A mid-level engineer compares the expected volume and operating cost in a short document, and suggests the existing managed queue. The senior engineer points out a replay requirement the document had missed. They agree on the managed queue plus an event archive, which meets the requirement at lower cost. Either ending works as a story, provided you can show what you learned.

See also [Conflict & collaboration](../../topics/10-behavioral/ANSWERS.md#conflict--collaboration).

#### Tell me about a time you had to balance a deadline against code quality.

**What they look for:** you make the trade-off on purpose, keep the risk visible and follow up. Neither "quality always wins" nor "we just shipped it" is a strong answer.

**STAR outline:**
- **S:** [The deadline, and why it could not move]
- **T:** [What could not be built properly in the time]
- **A:** [What you cut, what you kept (tests on critical paths, data integrity), who agreed, how you recorded the debt]
- **R:** [Shipped on time; the follow-up that was actually done]

**Example shape (invented, replace it with your own story):** A team has two weeks to launch a partner integration. The engineer proposes a simple synchronous flow with a manual retry tool rather than a full queue-based design. The team keeps idempotency and audit logging, because those cannot be added safely later. They agree a follow-up with the product manager, and the queue work is scheduled for the next sprint.

See also [Prioritisation & pressure](../../topics/10-behavioral/ANSWERS.md#prioritisation--pressure).

#### Tell me about something difficult you had to learn quickly.

**What they look for:** a repeatable way to learn: finding the right sources, building something small, asking good questions and applying the knowledge to real work.

**STAR outline:**
- **S:** [The new area and why it was needed quickly]
- **T:** [What you had to deliver, by when]
- **A:** [How you learned: official documentation, a small prototype, an expert's review, reading the source]
- **R:** [What you delivered; how your learning helped others]

**Example shape (invented, replace it with your own story):** An engineer who has never used Kafka joins a team whose consumer lag is growing. They read the documentation on consumer groups, build a small local test to see rebalancing in action, and ask a colleague to review their theory. They find that long-running processing is causing repeated rebalances, and the fix clears the lag. They then write a short guide for the team.

## Prep checklist

- [ ] Solve problems by pattern using [DS&A topics](../../topics/02-coding/ANSWERS-dsa-topics.md), timing yourself and speaking out loud.
- [ ] Implement an LRU cache, a token bucket and a retry helper from memory, with tests.
- [ ] Complete one low-level design in working code within 90 minutes, such as a [parking lot](../../topics/02-coding/ANSWERS.md#parking-lot) or the discount engine above.
- [ ] Be able to explain indexes, isolation levels, expand-and-contract migrations, the outbox pattern and idempotency without notes.
- [ ] Build a small service with an API, a database, a queue, tests, metrics and a deployment pipeline, so your answers come from practice.
- [ ] Read a real query plan with `EXPLAIN`, and fix a slow query on a table with a million rows.
- [ ] Practise five system design prompts on a timer, including one where you go deep on the data model.
- [ ] Prepare a 20-minute project deep dive with one diagram, your own decisions and what you would change.
- [ ] Prepare stories for a bug you caused, a disagreement, a deadline trade-off and fast learning, using the [story bank checklist](../../topics/00-overview/README.md#story-bank-checklist-prepare-one-story-for-each).
- [ ] Drill the [skills table's SDE row](../../skills/README.md#what-to-prepare-by-level), plus each tool named in the job description.
