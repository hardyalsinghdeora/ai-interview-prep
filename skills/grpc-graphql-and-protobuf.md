# gRPC, GraphQL & Protobuf

API style is a decision a team lives with for years, so interviewers use this topic to test whether you can evolve a contract safely and choose a protocol for a reason rather than a preference. Senior backend, Software Architect and Solutions Engineer roles ask most, usually as "REST, gRPC or GraphQL, and why?", followed by what breaks once the choice meets real clients and real traffic.

> Related: [API design & microservices](api-design-and-microservices.md) · [System design](../topics/03-system-design/README.md) · [Large-scale distributed systems](large-scale-distributed-systems.md) · [Kafka & event streaming](kafka-and-event-streaming.md)

---

## A. Fundamentals — must answer crisply
- [ ] What makes Protobuf smaller and faster than JSON, and what do you give up by using it?
- [ ] Why do Protobuf field numbers matter more than field names on the wire?
- [ ] Name the four gRPC call types and give one real use for each.
- [ ] What does gRPC gain from HTTP/2, and why can't a browser call a native gRPC service directly?
- [ ] What is a gRPC deadline, and how does it differ from a client-side timeout?
- [ ] What does a GraphQL resolver do, and how does the server execute a nested query?
- [ ] What is the N+1 problem in GraphQL, and why does the resolver model make it so easy to hit?
- [ ] How does GraphQL report an error, and why can a 200 response still contain a failure?
- [ ] REST vs. gRPC vs. GraphQL: give a one-line rule for where each fits best.

## B. Working depth
- [ ] Which Protobuf changes are safe, and which break clients? Cover adding, removing and renaming a field, changing a type, and using `reserved`.
- [ ] What is field presence in proto3, and how do you tell "not set" from a default value such as `0` or an empty string? *(verify)*
- [ ] Why should the zero value of a Protobuf enum be an `UNSPECIFIED` entry, and what happens to an enum value the reader does not recognise?
- [ ] How do you stream large results with gRPC, and how does flow control stop a fast server overwhelming a slow client?
- [ ] How should deadlines propagate across a chain of services, and what should a server do when a caller's deadline has already passed?
- [ ] Which gRPC status codes are safe to retry, and how do retry policies and hedging avoid amplifying an outage? *(verify)*
- [ ] Why does gRPC often load-balance badly behind a Layer 4 balancer, and how do you fix it with Layer 7 proxying, client-side balancing or a service mesh?
- [ ] What do ConnectRPC and Buf add to a plain `protoc` and gRPC setup, and what do you gain from linting and breaking-change checks in CI? *(verify)*
- [ ] How does DataLoader batch and cache within a request, and why must its cache not be shared across requests?
- [ ] Offset vs. cursor pagination in GraphQL: how do connections work, and what should nullable vs. non-null fields mean for partial failure?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] When would you adopt GraphQL, gRPC or REST for a public API vs. internal service traffic vs. a mobile client on a poor network?
- [ ] How do you protect a GraphQL endpoint from an expensive query: depth limits, cost analysis, timeouts, per-client budgets and persisted queries?
- [ ] Why is HTTP caching hard with GraphQL, and what do persisted queries, CDN caching and normalised client caches each solve?
- [ ] GraphQL federation vs. schema stitching vs. one monolithic graph: how do subgraphs, entities and the router work, and who owns the shared types? *(verify)*
- [ ] Field-level authorisation in GraphQL: where does it belong, and how do you stop an attacker reaching forbidden data through a different path in the graph?
- [ ] How do you evolve a GraphQL schema without versioning: deprecation, usage tracking and deciding when a field can finally be removed?
- [ ] How do you manage Protobuf schemas across many teams and languages: monorepo vs. schema registry, generated code ownership and breaking-change policy? *(verify)*
- [ ] Long-lived gRPC streams during deploys: how do you drain connections, rebalance after scale-out and resume a broken stream without losing data?
- [ ] How do you observe and debug gRPC when payloads are binary: tracing, per-method metrics, server reflection and its security implications? *(verify)*
- [ ] Protobuf for event payloads as well as RPC: what are the differences in compatibility requirements when messages may be read years later?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] A client deploy starts failing with parse errors after a "harmless" Protobuf change. Walk through the likely causes, the rollback and the CI check that would have caught it.
- [ ] After scaling a gRPC service from 3 to 12 pods, the original 3 still take nearly all the traffic. Explain why, and how you fix it.
- [ ] A GraphQL page that was fast now makes 800 database queries per request. How do you find the cause, fix it and stop it coming back?
- [ ] A single customer's GraphQL query takes the API down. Design the immediate response and a lasting query cost model.
- [ ] Design the API layer for a product with a web app, mobile apps, partner integrations and 40 internal services. Which protocol goes where?
- [ ] Migrate a set of internal REST services to gRPC or ConnectRPC without a coordinated release. What is your rollout plan?
- [ ] A team proposes a GraphQL gateway in front of a dozen microservices. Argue for and against, and state what would have to be true for you to agree.

## E. Hands-on checks (things to be able to DO)
- [ ] Write a `.proto` file for a small service, generate code, and show that an old client still works after a compatible schema change.
- [ ] Add `buf lint` and `buf breaking` to a CI pipeline, and show a breaking change being rejected. *(verify)*
- [ ] Implement a server-streaming gRPC method with deadlines and cancellation, and show the server stopping work when the client disconnects.
- [ ] Use `grpcurl` or `buf curl` to call and debug a running service from the command line. *(verify)*
- [ ] Build a GraphQL API with nested resolvers, log the query count, and fix an N+1 problem with DataLoader.
- [ ] Add a query cost limit to a GraphQL server, and show an abusive query being rejected before execution.

---

**Good answers show:**
- A choice driven by clients, network, team skills and tooling, with an honest case for plain REST when it is enough.
- A schema treated as a long-lived contract, with compatibility rules enforced in CI rather than by convention.
- Wire-level understanding of why renames, type changes and reused field numbers cause failures.
- Deadlines, cancellation and retries designed end to end, with attention to retry amplification.
- Awareness of operational costs: load balancing long-lived connections, binary debugging and caching.
- GraphQL safety as a design concern: batching, query cost limits, persisted queries and field-level authorisation.

**Prove it:** Build one small order service exposed three ways: REST, gRPC through ConnectRPC, and a GraphQL gateway using DataLoader. Enforce Protobuf compatibility with Buf in CI, add query cost limits, and load test all three with the same data. Write up the measured latency and payload size, what a schema change involved in each, and which style you would pick for three named kinds of client.
