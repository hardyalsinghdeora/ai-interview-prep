# API Design Cheatsheet

Covers REST constraints, HTTP semantics, idempotency, pagination, versioning, webhooks, gRPC and protobuf, GraphQL, gateways, rate limiting, OpenAPI and contract testing.

> Related: [API design and microservices skill](../skills/api-design-and-microservices.md) · [System design](../topics/03-system-design/README.md) · [Cloud, DevOps and integration](../topics/08-cloud-devops-integration/README.md) · [security-and-auth.md](security-and-auth.md) · [networking-and-web.md](networking-and-web.md) · [messaging-and-streaming.md](messaging-and-streaming.md)

## REST constraints

| Term | What it means |
|---|---|
| **REST** (Representational State Transfer) | An architectural style for networked systems built around resources, representations and a uniform interface. |
| **Client-server** | Separates user interface concerns from data storage, so each can evolve independently. |
| **Statelessness** | Each request carries everything needed, so servers can scale without storing client session state. |
| **Cacheability** | Responses state whether they can be cached, reducing latency and server load. |
| **Uniform interface** | Standard resource identifiers and methods make APIs predictable across clients. |
| **Layered system** | Clients cannot tell whether proxies or gateways sit in the path, allowing infrastructure to be added freely. |
| **Code on demand** | The optional constraint: servers may send executable code to clients. |
| **HATEOAS** (Hypermedia as the Engine of Application State) | Responses include links to next actions, so clients discover flows; rarely fully applied in practice. |
| **Resource** | Any named thing an API exposes, addressed by URL, usually as a noun such as `/orders/42`. |

## HTTP semantics

| Term | What it means |
|---|---|
| **Safe method** | A request that should not change server state, such as `GET` and `HEAD`. |
| **Idempotent method** | Repeating has the same intended effect as sending once, as with `GET`, `PUT` and `DELETE`. |
| **`POST`** | Creates or triggers an action; neither safe nor idempotent by default. |
| **`PUT`** | Replaces a resource's full representation at a known URL; idempotent. |
| **`PATCH`** | Applies a partial update; not guaranteed to be idempotent. |
| **`201 Created`** | A resource was created; commonly returned with a `Location` header. |
| **`202 Accepted`** | Work was queued but not finished, which suits long-running operations. |
| **`401` vs `403`** | `401` means not authenticated; `403` means authenticated but not permitted. |
| **`409 Conflict`** | The request conflicts with current state, for example a version clash. |
| **`422 Unprocessable Content`** | Syntax is fine but the content fails validation. |
| **Problem Details** | RFC 9457 standard JSON format for HTTP API errors, which gives clients consistent handling *(verify)*. |
| **ETag and `If-Match`** (Entity Tag) | Conditional requests that provide optimistic concurrency, preventing lost updates. |

| Method | Safe | Idempotent |
|---|---|---|
| `GET` | Yes | Yes |
| `PUT` | No | Yes |
| `DELETE` | No | Yes |
| `POST` | No | No |
| `PATCH` | No | Not guaranteed |

## Idempotency and long-running work

| Term | What it means |
|---|---|
| **Idempotency** | Safe retries produce one outcome, which is essential when networks fail after a request is processed. |
| **Idempotency key** | A client-generated unique ID on a request, allowing the server to detect and answer retries. |
| **Stored response** | The saved outcome returned on repeat requests, usually kept for a limited period. |
| **Request fingerprint** | A hash of the request body, used to reject reuse of a key with different data. |
| **Asynchronous request-reply** | Returns `202` with a status URL to poll, which avoids long-held HTTP connections. |

```http
POST /payments
Idempotency-Key: 7f3a9c1e-...
```

## Pagination, filtering and versioning

| Term | What it means |
|---|---|
| **Offset pagination** | Uses `limit` and `offset`; simple and allows page jumps, but slows and drifts on large, changing data. |
| **Cursor pagination** | Returns an opaque token for the next page, staying stable and efficient as data changes. |
| **Keyset pagination** | Filters from the last seen sort value, using an index; the common basis for cursors. |
| **Sparse fieldsets** | Lets clients request only certain fields, which cuts payload size. |
| **URI versioning** (Uniform Resource Identifier versioning) | Puts the version in the path, such as `/v1/orders`; it is explicit and easy to route. |
| **Header versioning** | Selects the version through a request header, keeping URLs clean but making testing less obvious. |
| **Breaking change** | Anything that can fail existing clients, such as removing or renaming a field. |
| **Additive change** | Adds optional fields or endpoints, which is normally backward compatible. |
| **Tolerant reader** | A client ignoring unknown fields, which lets APIs evolve without breaking it. |
| **`Deprecation` and `Sunset` headers** | Signal to clients that an API is deprecated and when it will be removed. |

| Offset vs cursor | Offset | Cursor |
|---|---|---|
| Jump to page | Yes | No |
| Performance when deep | Degrades | Stable |
| Consistency during inserts | Skips or repeats | Stable |

## Webhooks

| Term | What it means |
|---|---|
| **Webhook** | An HTTP call a provider makes to your endpoint when an event occurs, replacing constant polling. |
| **Webhook signature** | An HMAC or similar signature over the payload, proving the sender and that it was not altered. |
| **Replay protection** | Rejects old or repeated deliveries, usually by checking a signed timestamp and event ID. |
| **Webhook retry** | Providers resend after failures, often with backoff, so receivers must handle duplicates. |
| **Acknowledge then process** | Return `2xx` quickly and queue the work, which avoids timeouts and unnecessary retries. |
| **Event ordering** | Webhooks may arrive out of order, so use timestamps or fetch current state. |

## gRPC and protobuf

| Term | What it means |
|---|---|
| **RPC** (Remote Procedure Call) | Calling a remote function as if it were local, while still handling network failure. |
| **gRPC** (gRPC Remote Procedure Calls) | A high-performance RPC framework on HTTP/2 with protobuf and generated clients, popular between internal services. |
| **Protobuf** (Protocol Buffers) | A compact binary serialisation format with a schema, smaller and faster to parse than JSON. |
| **Field number** | Identifies a protobuf field on the wire, so it must never be changed or reused. |
| **`reserved`** | Blocks reuse of deleted protobuf field numbers or names, protecting compatibility. |
| **Unary RPC** (unary Remote Procedure Call) | One request and one response, the simplest call type. |
| **Server streaming** | One request produces many responses over time. |
| **Client streaming** | Many requests are sent, then one response returns. |
| **Bidirectional streaming** | Both sides send independent message streams on one call. |
| **Deadline** | A client's total time limit, propagated across calls, which stops work nobody is waiting for. |
| **gRPC-Web** | Lets browsers call gRPC services through a proxy, since browsers lack the needed HTTP/2 control. |

| API style | Strength | Weakness | Best for |
|---|---|---|---|
| REST | Simple, cacheable, universal | Over- or under-fetching | Public APIs |
| gRPC | Fast, typed, streaming | Limited browser support | Internal services |
| GraphQL | Client-shaped queries | Caching and query cost control | Varied front ends |

## GraphQL

| Term | What it means |
|---|---|
| **GraphQL** (Graph Query Language) | A typed query language where clients ask for exactly the fields they need from one endpoint. |
| **GraphQL schema** | The typed contract listing available data and operations. |
| **Query** | A read operation in GraphQL. |
| **Mutation** | A write operation in GraphQL. |
| **Subscription** | A long-lived operation pushing updates to clients, often over WebSockets. |
| **Resolver** | The function that fetches data for a field; careless resolvers cause inefficient data access. |
| **N+1 problem** | One query for a list, then one per item, which floods the backend with calls. |
| **DataLoader** | Batches and caches lookups within a request, which fixes the N+1 problem. |
| **Persisted query** | Sends a known query's ID rather than its text, improving safety, caching and payload size. |
| **Query complexity limit** | Rejects overly deep or expensive queries, protecting the server from abuse. |
| **Federation** | Combines subgraphs owned by different teams into one graph. |

## Gateways and rate limiting

| Term | What it means |
|---|---|
| **API gateway** (Application Programming Interface gateway) | A single entry point handling routing, auth, rate limits and logging, keeping these out of services. |
| **BFF** (Backend for Frontend) | A dedicated backend per client type, which tailors APIs for web and mobile separately. |
| **Rate limiting** | Caps request volume per client, which protects capacity and ensures fairness. |
| **Throttling** | Slowing or rejecting requests above a limit, often used interchangeably with rate limiting. |
| **Quota** | A usage allowance over a longer period, such as per month, often tied to a plan. |
| **Token bucket** | Tokens refill steadily and requests spend them, which allows short bursts within an average rate. |
| **Leaky bucket** | Requests leave a queue at a fixed rate, which smooths traffic but limits bursts. |
| **Fixed window** | Counts per fixed time slot; simple, but allows double bursts at window edges. |
| **Sliding window** | Counts over a rolling period, which is smoother at some extra cost. |
| **`429 Too Many Requests`** | The rate limit response, ideally with a `Retry-After` header for clients. |

## Specs and contract testing

| Term | What it means |
|---|---|
| **OpenAPI** | A machine-readable description of HTTP APIs, used for documentation, validation and client generation. |
| **AsyncAPI** | A similar specification for event-driven APIs and message channels. |
| **Design-first** | Writing the API spec before code, which enables early feedback and parallel work. |
| **Contract test** | Checks that a service and its client agree on an API, without full end-to-end environments. |
| **Consumer-driven contract** | Consumers state what they need, and providers verify against it, as in Pact. |
| **Provider verification** | Runs consumer expectations against the provider, typically in its CI pipeline. |
| **Schema diff check** | Compares specs automatically in CI, flagging breaking changes before release. |
