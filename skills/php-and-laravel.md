# PHP & Laravel

PHP still runs a large share of the web, and many companies have profitable Laravel or legacy PHP systems they need to scale, modernise or gradually replace. Interviewers ask to see whether you understand the request-per-process execution model and can make sound decisions about old code rather than simply calling for a rewrite. Senior backend roles at established product companies ask most, along with solutions and Forward Deployed Engineer roles where the customer's existing estate is PHP.

> Related: [Data migration & legacy modernisation](data-migration-and-legacy-modernisation.md) · [PostgreSQL & data modelling](postgresql-and-data-modelling.md) · [Node.js & TypeScript](nodejs-and-typescript.md) · [Go](go.md)

---

## A. Fundamentals — must answer crisply
- [ ] Describe the lifecycle of a PHP request under PHP-FPM. What is shared between requests, and what is not?
- [ ] How does PHP's shared-nothing model compare with a long-running Node.js or Go process?
- [ ] What is OPcache, what does it store, and why does it matter so much in production?
- [ ] How do `composer.json` and `composer.lock` differ, and which do you commit and deploy from?
- [ ] What does PSR-4 autoloading do, and how does Composer use it?
- [ ] What is Laravel's service container, and how does automatic dependency injection resolve a class?
- [ ] What is the N+1 query problem in Eloquent, and how does eager loading fix it?
- [ ] Migrations, seeders and factories: what does each do, and how are they used in a team?
- [ ] Walk through a Laravel request from `public/index.php` through middleware, routing and controller to response.

## B. Working depth
- [ ] PHP-FPM pool settings: `pm` modes, `pm.max_children` and `pm.max_requests`. How do you size a pool from memory use?
- [ ] OPcache in deployment: `validate_timestamps`, preloading, and how a symlink deploy can serve stale code. *(verify)*
- [ ] `bind` vs. `singleton` vs. contextual binding; what are service providers for, and what is the difference between `register` and `boot`?
- [ ] `with` vs. `load` vs. `chunk` vs. `cursor` vs. `lazy` in Eloquent: which do you use for a large export, and why? *(verify)*
- [ ] How do you detect N+1 queries before production, for example with `Model::preventLazyLoading`, Telescope or query logging? *(verify)*
- [ ] Eloquent mass assignment, `$fillable` and `$guarded`: what is the vulnerability, and how do form requests help?
- [ ] Laravel queues: how do jobs, workers, retries, backoff, `failed_jobs` and Horizon fit together? *(verify)*
- [ ] Why must queue workers be restarted on deploy, and what memory and state problems come with long-running workers?
- [ ] Testing in Laravel: feature vs. unit tests, `RefreshDatabase`, HTTP fakes and queue fakes, and PHPUnit vs. Pest. *(verify)*
- [ ] Which PHP language features most improve an old codebase: strict types, typed properties, enums, readonly, attributes? *(verify)*
- [ ] How do you find and handle security issues in PHP: SQL injection, unsafe deserialisation, file uploads and `composer audit`? *(verify)*

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] How would you plan a major PHP version migration, including deprecations, extension and dependency compatibility, and tools such as Rector and PHPStan? *(verify)*
- [ ] Laravel Octane, Swoole and FrankenPHP change the execution model. What gets faster, and what new bugs appear from shared state? *(verify)*
- [ ] Active Record (Eloquent) vs. a data mapper (Doctrine): where does Eloquent start to hurt in a large domain?
- [ ] How do you scale a Laravel app horizontally: sessions, cache, file storage, scheduler and database read replicas?
- [ ] Connection handling under FPM: why does a large pool overwhelm a database, and where does a pooler such as PgBouncer or ProxySQL fit?
- [ ] How would you run schema changes on a large MySQL or PostgreSQL table in a Laravel app without downtime? *(verify)*
- [ ] How do you modernise a legacy PHP app with no tests: characterisation tests, the strangler fig pattern, and where to draw the first boundary?
- [ ] When is moving a service from PHP to Node.js or Go justified, and when is upgrading PHP the better investment?
- [ ] What does a migration from PHP really cost in lost business rules, team skills, hiring and running two stacks at once?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Under load, Nginx returns 502s and the FPM log says `max_children` was reached. How do you diagnose it and what do you change?
- [ ] An admin report takes 40 seconds and uses 1 GB of memory. How do you find the cause and fix it in Eloquent?
- [ ] A queue job sometimes runs twice and charges a customer twice. Why can that happen, and how do you make it safe?
- [ ] You inherit a PHP 7 app with no framework, no tests and raw SQL everywhere. Plan the first 90 days. *(verify)*
- [ ] Plan the extraction of a high-traffic service from a Laravel monolith to Go or Node.js: routing, data ownership, shadow traffic, cutover and rollback.
- [ ] After a deploy, some servers run old code and queue jobs fail on a changed payload. Explain why, and design a safer release process.

## E. Hands-on checks (things to be able to DO)
- [ ] Configure Nginx and PHP-FPM with OPcache, and calculate `pm.max_children` from measured memory per worker.
- [ ] Build a small Laravel API with form requests, policies, API resources and feature tests.
- [ ] Find and fix an N+1 query using a query log or Telescope, with a test that asserts the query count.
- [ ] Run a queued job with retries and a failure handler under Horizon or `queue:work`, and show it surviving a worker restart.
- [ ] Run PHPStan and Rector on an older codebase, and upgrade it one PHP version with the tests passing. *(verify)*

---

**Good answers show:**
- Reasoning from the execution model: what resets per request, what persists, and how that shapes performance.
- Respect for working legacy code, and incremental change backed by tests rather than a reflex to rewrite.
- Practical Laravel knowledge, including when its conveniences hide query or memory costs.
- Production awareness: FPM sizing, OPcache behaviour, worker restarts and safe schema changes.
- Migration decisions justified by evidence, such as measured bottlenecks, cost and team skills, rather than language preference.
- Reliability in background work, with idempotent jobs, sensible retries and visibility into failures.

**Prove it:** Take a small Laravel app, load-test it under PHP-FPM and tune its pool and OPcache settings, then fix a real N+1 query and add a queued job that is idempotent. Upgrade it one major PHP version with Rector and PHPStan. Finally, extract one endpoint to Go or Node.js behind the same proxy using shadow traffic, and write up the performance difference and whether the move was worth it.
