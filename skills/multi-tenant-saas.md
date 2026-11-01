# Multi-tenant SaaS

Multi-tenancy decisions shape a SaaS product's data model, cost structure and sales options for years, and one cross-tenant data leak can end a customer relationship. Interviewers use this topic to test whether you can balance isolation against cost and explain what happens when one customer outgrows the rest. Senior backend, Software Architect, Solutions Architect and Forward Deployed Engineer roles ask most, because enterprise customers routinely ask for exceptions to the standard model.

> Related: [System design](../topics/03-system-design/README.md) · [PostgreSQL & data modelling](postgresql-and-data-modelling.md) · [Authentication & authorisation](authentication-and-authorization.md) · [Billing, payments & order-to-cash](billing-payments-and-order-to-cash.md)

---

## A. Fundamentals — must answer crisply
- [ ] What is a tenant, and how does it differ from a user, an account or an organisation in your data model?
- [ ] Silo vs. pool vs. bridge: define each isolation model and give one reason to pick it.
- [ ] Shared tables with a `tenant_id` vs. schema-per-tenant vs. database-per-tenant: how do isolation, cost and operations compare?
- [ ] What is a noisy neighbour, and which shared resources does it usually affect first?
- [ ] Where should tenant context come from on a request, and why should a service never trust a tenant ID supplied in the request body?
- [ ] What can go wrong if tenant filtering depends on every developer remembering a `WHERE` clause?
- [ ] Rate limits vs. quotas: what is the difference, and what should a client see when it hits each?
- [ ] What does data residency mean, and how does it differ from data sovereignty?
- [ ] Which features and guarantees usually separate a standard tier from an enterprise tier?

## B. Working depth
- [ ] PostgreSQL row-level security: how do you set it up, and how do you pass tenant context safely when using a transaction-pooling connection pooler? *(verify)*
- [ ] How do you include tenant context in caches, search indexes, object storage paths and message queues so that isolation holds outside the main database?
- [ ] Tenant-aware routing: how do you map a tenant to a shard, region or cell through subdomains, tokens or a catalogue service? What happens when that lookup is down?
- [ ] How do you enforce per-tenant limits across many API nodes, and which algorithm would you choose for bursty traffic? Where does the counter live?
- [ ] How do you stop one tenant dominating background workers and database connections?
- [ ] Should every table carry `tenant_id`? How does it affect primary keys, indexes, foreign keys and partitioning?
- [ ] How do you model a hierarchy such as agency, sub-account and user, including which data, settings and billing are inherited and which are not?
- [ ] An agency user needs access to 200 sub-accounts. How do you model that access, switch context safely and show it in the audit log?
- [ ] How do you manage per-tenant configuration and feature flags without creating a separate code path for every customer?
- [ ] How do you support tenant onboarding, data export and full tenant deletion, including backups and downstream systems?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] How do you decide when a tenant should move from the shared pool to dedicated infrastructure, and how do you move it without downtime?
- [ ] Cell-based architecture: how does it limit blast radius, how do you size a cell, and what does it make harder?
- [ ] How do you run schema migrations across thousands of tenant databases or schemas: ordering, partial failure, version drift and rollback? *(verify)*
- [ ] How do you roll out a release in stages by tenant, and when is it acceptable for tenants to run different versions?
- [ ] One tenant holds 40% of the data in a shared table. What breaks first, and what are your options for rebalancing or sharding it?
- [ ] How do you meet EU-only data residency when support tooling, logs, backups, analytics and LLM or third-party calls all handle customer data? *(verify)*
- [ ] Customer-managed encryption keys: how do they work per tenant, and what happens when a customer revokes a key? *(verify)*
- [ ] How do you attribute shared infrastructure costs to each tenant, and which metering signals are accurate enough to inform pricing? *(verify)*
- [ ] Which metrics and alerts must be broken down per tenant, and how do you avoid a cardinality explosion in your observability bill? *(verify)*
- [ ] How do you test tenant isolation continuously, and how would you detect a cross-tenant leak in production?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] One tenant's nightly export makes the product slow for everyone else. How do you diagnose it now, and what do you change so that it cannot recur?
- [ ] A cache key missing a tenant ID showed one customer's data to another. Walk through the incident response, the customer communication and the structural fix.
- [ ] Design a multi-tenant SaaS product with agencies, sub-accounts, per-sub-account billing and white-labelling.
- [ ] Your largest prospect demands a dedicated database, EU hosting and customer-managed keys. How do you evaluate the request, and how do you deliver it without forking the product?
- [ ] Migrate a single-tenant, per-customer deployment estate of 300 installations into a shared multi-tenant platform.
- [ ] Add a new column with a backfill to a table of 5 billion rows across shards, with some tenants in change-freeze windows.
- [ ] Design a usage-metering and quota system that can throttle a tenant within seconds and also produce accurate invoices.

## E. Hands-on checks (things to be able to DO)
- [ ] Enable PostgreSQL row-level security on a shared table, and write a test proving that one tenant cannot read another's rows, even with a query that omits the filter.
- [ ] Write middleware that derives the tenant from a verified token and carries it through to the database session, logs and queued jobs.
- [ ] Implement a per-tenant token-bucket limiter in Redis, and show it holding under a load test across several API instances.
- [ ] Build a weighted fair queue for background jobs, and show a small tenant's latency staying stable while a large one floods the system.
- [ ] Write a migration runner for many tenant schemas that records progress per tenant and can resume after a failure.
- [ ] Produce a per-tenant cost report from tagged infrastructure costs and application usage metrics.

---

**Good answers show:**
- Isolation chosen by risk and customer need, usually a hybrid, rather than one model applied everywhere.
- Tenant context enforced by the platform at several layers, so that a forgotten filter cannot become a data leak.
- Fairness designed in: limits, quotas and work scheduling, with clear behaviour for a tenant that exceeds them.
- Attention to the awkward data paths: caches, logs, backups, analytics, support tools and third-party services.
- Operational realism about migrations, staged rollouts and moving a tenant while it stays online.
- Commercial awareness: cost per tenant, tier design, and when an enterprise request is worth the permanent complexity.

**Prove it:** Build a small multi-tenant API on PostgreSQL with row-level security, an agency and sub-account hierarchy, per-tenant Redis rate limits and fair job scheduling. Add a tool that moves one tenant from the shared database to a dedicated one while it stays online, plus an automated cross-tenant leak test in CI. Load test with one deliberately noisy tenant, then publish the latency for the others and the per-tenant cost estimates.
