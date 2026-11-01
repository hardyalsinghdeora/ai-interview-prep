# .NET & C#

Enterprise, finance, healthcare and public-sector teams run a lot of .NET, much of it midway between legacy .NET Framework applications and modern cross-platform services. Interviewers want to see that you understand the runtime, async and data access well enough to keep those systems fast, and can plan a migration without stopping delivery. Senior backend, Software Architect and Tech Lead roles in Microsoft-centred organisations ask most, often framed as "how would you modernise this ten-year-old application?".

> Related: [Azure](azure.md) · [Java & Spring Boot](java-and-spring-boot.md) · [API design & microservices](api-design-and-microservices.md) · [Data migration & legacy modernisation](data-migration-and-legacy-modernisation.md) · [Docker & CI/CD](docker-and-ci-cd.md)

---

## A. Fundamentals — must answer crisply
- [ ] What are the CLR, IL and the JIT, and what happens between `dotnet build` and your code running?
- [ ] Value types vs. reference types: how do they differ in copying and allocation, and what is boxing?
- [ ] How does the generational garbage collector work (generations 0, 1 and 2, and the large object heap)?
- [ ] `IDisposable`, `using` and finalisers: what does each do, and when do you actually need a finaliser?
- [ ] What does the compiler do with `async` and `await`, and how is asynchrony different from parallelism?
- [ ] `IEnumerable<T>` vs. `IQueryable<T>`: what does deferred execution mean for each?
- [ ] Generics: what do constraints give you, and how do .NET's reified generics differ from Java's type erasure?
- [ ] Transient, scoped and singleton lifetimes: what does each mean, and what is a captive dependency?
- [ ] How does the ASP.NET Core middleware pipeline work (`Use`, `Map`, `Run`), and why must `UseAuthentication` come before `UseAuthorization`?
- [ ] Classes, structs and records: how do you choose, and what do nullable reference types actually enforce? *(verify)*

## B. Working depth
- [ ] What do `SynchronizationContext` and `ConfigureAwait(false)` do, and why can `.Result` deadlock in some application models but not usually in ASP.NET Core?
- [ ] `Task` vs. `ValueTask`, `CancellationToken` propagation, `async void`, and how exceptions surface from `Task.WhenAll`.
- [ ] LINQ pitfalls: multiple enumeration, allocations in hot paths, and calling `AsEnumerable` or `ToList` too early against a database.
- [ ] Explain variance in generics (`in` and `out`). How do `IEnumerable<out T>` and `Action<in T>` make an API easier to use?
- [ ] When does a generic repository over Entity Framework Core help, and when does it just hide useful features?
- [ ] Middleware vs. MVC filters vs. endpoint filters: which would you use for logging, validation and authorisation, and where does `[Authorize]` fit? *(verify)*
- [ ] Controllers vs. Minimal APIs: what would you choose for a new service, and what do you gain or lose? *(verify)*
- [ ] Entity Framework Core change tracking: `AsNoTracking`, `Include`, split queries, and how N+1 queries still happen.
- [ ] Why is `DbContext` not thread-safe, what does `AddDbContextPool` change, and how do you use it correctly from a `BackgroundService`?
- [ ] `IOptions`, `IOptionsSnapshot` and `IOptionsMonitor`: what is the difference, and how do you keep secrets out of configuration files?
- [ ] Testing: xUnit, `WebApplicationFactory`, Testcontainers, and why the in-memory database provider can mislead you.

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] Workstation vs. server GC, background GC and dynamic heap sizing: how do you choose, and what do you measure first? *(verify)*
- [ ] How do you diagnose a production memory leak with `dotnet-counters`, `dotnet-gcdump`, `dotnet-dump` and `dotnet-trace`?
- [ ] What causes thread pool starvation, what does it look like in metrics, and why is adding more threads rarely the real fix?
- [ ] `Span<T>`, `Memory<T>`, `ArrayPool<T>` and `stackalloc`: when is reducing allocations worth the extra complexity, and how do you prove it with BenchmarkDotNet?
- [ ] Native AOT vs. ReadyToRun vs. tiered compilation: what are the trade-offs in startup, throughput and reflection-heavy libraries? *(verify)*
- [ ] Entity Framework Core at scale: compiled queries, `ExecuteUpdate`, concurrency tokens, and when to switch to Dapper or plain SQL. *(verify)*
- [ ] What blocks a move from .NET Framework (`System.Web`, WCF, Web Forms, AppDomains, Windows-only libraries), and which upgrade tools would you evaluate? *(verify)*
- [ ] LTS vs. STS releases: how do you keep many services on supported versions without a costly upgrade each year? *(verify)*
- [ ] Framework-dependent vs. self-contained vs. single-file deployment, container base images, and how the GC responds to container memory limits. *(verify)*
- [ ] `HttpClient` lifetime problems, `IHttpClientFactory` and resilience policies: what fails under load, and what would you standardise across teams? *(verify)*

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] API latency climbs under load, CPU stays moderate and the thread pool queue keeps growing. Walk through your investigation and the likely fix.
- [ ] An order page takes 8 seconds because a `Where` runs after `AsEnumerable` and related rows load one at a time. Find it and rewrite the query.
- [ ] A hosted service that injects a scoped `DbContext` throws intermittent errors in production. Explain what is wrong and how to fix and prevent it.
- [ ] Plan the migration of a large ASP.NET MVC 5 and WCF application from .NET Framework while features keep shipping. What moves first, and how do you run old and new side by side?
- [ ] Design an order-processing service using `BackgroundService`, an outbox table and Entity Framework Core, with at-least-once delivery and graceful shutdown.
- [ ] Multiple instances of a service must roll out a breaking Entity Framework Core schema change with no downtime. How do you generate, apply and sequence the migration?

## E. Hands-on checks (things to be able to DO)
- [ ] Build an ASP.NET Core API with validation, problem-details errors, and Entity Framework Core migrations against a real database.
- [ ] Write custom middleware that adds a correlation ID, and prove its ordering with an integration test using `WebApplicationFactory`.
- [ ] Reproduce thread pool starvation with sync-over-async code and a load test, observe it in `dotnet-counters`, then fix it.
- [ ] Compare a LINQ pipeline with a `Span<T>`-based version in BenchmarkDotNet, and explain the time and allocation results.
- [ ] Capture a GC dump from a running service, find what is retaining memory, and fix the leak.
- [ ] Publish the service with a multi-stage container build, run it under a memory limit, and generate an idempotent migration script for deployment.

---

**Good answers show:**
- A clear model of the runtime: allocation, GC, the thread pool and what `await` really does.
- Async done properly from top to bottom, with cancellation and no sync-over-async shortcuts.
- Entity Framework Core pragmatism: reading the generated SQL, controlling tracking and knowing when to step outside it.
- Performance work led by counters, traces and benchmarks rather than folklore or premature optimisation.
- Migration plans that keep delivering value, using incremental steps and side-by-side running instead of a big-bang rewrite.
- Deployment choices tied to real constraints such as startup time, image size, memory limits and support windows.

**Prove it:** Build a small orders API in ASP.NET Core with Entity Framework Core, Testcontainers tests and OpenTelemetry metrics, then add one deliberate performance problem such as an N+1 query or sync-over-async call. Load-test it, diagnose it with the .NET tools, fix it, and write a one-page note with the before-and-after numbers.
