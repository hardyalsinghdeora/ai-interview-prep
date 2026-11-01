# Django, FastAPI & Flask

Python web teams rarely ask about one framework in isolation: they want to know that you can choose between a batteries-included framework, an async-first API layer and a minimal core, then run the result in production without N+1 queries, blocked event loops or lost background jobs. Backend, Full-Stack, AI Platform and Tech Lead roles in Python-heavy organisations ask most, often framed as "which would you pick for this service, and what would you regret later?".

> Related: [Python](python.md) · [PostgreSQL & data modelling](postgresql-and-data-modelling.md) · [Task queues](rabbitmq-sqs-and-task-queues.md) · [Authentication & authorisation](authentication-and-authorization.md) · [Docker & CI/CD](docker-and-ci-cd.md)

---

## A. Fundamentals — must answer crisply
- [ ] What does each of Django, FastAPI and Flask give you out of the box, and what do you have to add yourself?
- [ ] Walk through a Django request from URL routing to response: middleware, view, ORM and template or serialiser.
- [ ] What is the N+1 problem in the Django ORM, and what is the difference between `select_related` and `prefetch_related`?
- [ ] QuerySets are lazy: which operations actually hit the database, and when are results cached?
- [ ] What do `makemigrations` and `migrate` each do, and how does Django know which migrations have been applied?
- [ ] What is Django middleware, and why does the order of the `MIDDLEWARE` setting matter?
- [ ] WSGI vs. ASGI: what does each interface define, and which servers would you run for each?
- [ ] In FastAPI, how is an `async def` route executed differently from a plain `def` route?
- [ ] What does Pydantic do for a FastAPI application across validation, serialisation and OpenAPI generation?
- [ ] What are Flask's application factory, blueprints, and application and request contexts?

## B. Working depth
- [ ] Where should business logic live in a Django project (models, managers, services, views), and how do you stop it leaking into serialisers?
- [ ] How do `annotate`, `aggregate`, `F`, `Q` and `Subquery` work, and when would you drop to raw SQL instead?
- [ ] Explain `transaction.atomic`, `select_for_update`, `ATOMIC_REQUESTS` and `transaction.on_commit`. What goes wrong without `on_commit`?
- [ ] Data migrations with `RunPython`: how do you keep them reversible, and how do you resolve conflicting migrations from two branches?
- [ ] How does FastAPI's `Depends` work, and how would you manage a request-scoped database session with a `yield` dependency?
- [ ] Pydantic validators, `response_model` and separate input, output and ORM models: how do they stop you exposing internal fields? *(verify)*
- [ ] How does the Flask extension pattern (`init_app`) work, and how do Flask-SQLAlchemy, Flask-Migrate and Flask-Login fit together? How would you assess an extension that looks unmaintained?
- [ ] Flask accepts `async def` views. Why does that not make it an ASGI framework, and when would you consider Quart instead? *(verify)*
- [ ] Describe Celery's moving parts (broker, worker, result backend, beat) and what `acks_late`, retries and the prefetch multiplier change.
- [ ] Session cookies with CSRF protection vs. JWTs: how do expiry, storage, refresh rotation and revocation differ between a Django app and a FastAPI API?
- [ ] Testing: `TestCase` vs. `TransactionTestCase`, `pytest-django`, FastAPI's `TestClient` with `dependency_overrides`, and Flask's `test_client`.

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] How would you choose a framework for a new service, weighing admin needs, ORM maturity, async workload, team skills and long-term maintenance?
- [ ] How much of Django is natively async today, what does `sync_to_async` cost, and when does moving to ASGI bring a real gain? *(verify)*
- [ ] Zero-downtime Django migrations: `AddIndexConcurrently`, `atomic = False`, lock timeouts, expand/contract, and old code running against the new schema.
- [ ] Database connections under load: `CONN_MAX_AGE`, built-in pooling, PgBouncer in transaction mode and SQLAlchemy pool sizing. What breaks when they are combined carelessly? *(verify)*
- [ ] Gunicorn sync and `gthread` workers vs. Uvicorn workers: how do you size them, and how do the GIL and free-threaded Python affect the decision? *(verify)*
- [ ] Celery failure modes: duplicate delivery from a Redis visibility timeout, poison messages, long tasks, queue isolation and worker shutdown during a deploy.
- [ ] Would you adopt Django's built-in tasks framework or stay on Celery, and what would drive that choice? *(verify)*
- [ ] How do you detect event-loop blocking in a production FastAPI service, and where should CPU-bound work go instead?
- [ ] What security protections does Django provide by default, and what must you build yourself in FastAPI or Flask (CSRF, object-level authorisation, rate limiting, secure headers)?
- [ ] What would you wire in on day one for observability, including tracing that follows a request into a Celery task? *(verify)*

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] A Django REST Framework list endpoint jumps from 12 queries to 800 after a nested serialiser is added. Find it, fix it and stop it from coming back.
- [ ] A FastAPI service's p99 latency spikes while CPU stays low, and one `async def` route calls a synchronous HTTP client. How do you prove the cause and fix it safely?
- [ ] Add a non-null column with a computed backfill to a 200-million-row table behind a live Django app. Walk through each release.
- [ ] Design a report-export feature with Celery: no duplicate emails, progress updates, retries, and no task that starts before its database row is committed.
- [ ] A large Flask monolith needs typed APIs and better concurrency. Would you migrate to FastAPI incrementally, adopt Django, or stay put? How would you make the case?
- [ ] A Django admin back office and a public FastAPI API must share users and permissions. Design the token, session and authorisation model.
- [ ] Plan a rolling deployment where web processes, Celery workers and a schema change all ship together. What order do you use, and how do you handle tasks queued by the old version?

## E. Hands-on checks (things to be able to DO)
- [ ] Build the same small CRUD API in Django REST Framework and FastAPI, then compare code, generated docs and test setup.
- [ ] Reproduce an N+1 query with Django Debug Toolbar, fix it, and lock in the fix with `assertNumQueries`.
- [ ] Write Django middleware that adds a request ID and logs timing, place it correctly, and show it working with a test.
- [ ] Write a Celery task with backoff, an idempotency key and `transaction.on_commit`, then kill the worker mid-task and show that the result is still correct.
- [ ] Build a Flask app with an application factory, a blueprint, Flask-SQLAlchemy and Flask-Migrate, and test it with `test_client`.
- [ ] Containerise a service behind Gunicorn or Uvicorn with health checks and graceful shutdown, and run `manage.py check --deploy` for the Django version.

---

**Good answers show:**
- Framework choice argued from the workload and team, not from preference or benchmark headlines.
- A habit of looking at the SQL: knowing when QuerySets run, and preventing N+1 queries with tests rather than review alone.
- A correct model of sync, threads and the event loop, including what blocks what under WSGI and ASGI.
- Migration discipline on live data: small steps, lock awareness and code that works with both schemas.
- Background work designed for at-least-once delivery, with idempotency and clear transaction boundaries.
- Production detail that comes from running these systems: worker sizing, connection pooling, graceful shutdown and tracing.

**Prove it:** Build a small invoices service twice, once in Django with Django REST Framework and once in FastAPI with SQLAlchemy, both on PostgreSQL, with a Celery task that emails a PDF and tests that assert query counts. Load-test both, then write a one-page note comparing latency, memory, code volume and which you would choose for which kind of team.
