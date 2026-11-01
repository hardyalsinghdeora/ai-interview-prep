# Service Mesh & API Gateways

Gateways and meshes sit in the path of every request, so a bad timeout, retry policy or certificate rotation can take down an otherwise healthy system. Interviewers ask about them to see whether you understand what the proxy layer is really doing and whether you can judge when it is worth its operational cost. Platform, Staff/Principal backend, solutions and Forward Deployed Engineer roles ask most, because customers often already run a gateway or mesh you must integrate with.

> Related: [Cloud, DevOps & Integration](../topics/08-cloud-devops-integration/README.md) · [Kubernetes](kubernetes.md) · [API design & microservices](api-design-and-microservices.md) · [Linux & networking](linux-and-networking.md)

---

## A. Fundamentals — must answer crisply
- [ ] Reverse proxy vs. API gateway vs. service mesh — what problem does each solve, and where do they overlap?
- [ ] North-south vs. east-west traffic: which tools usually handle which, and why?
- [ ] What can an L7 proxy do that an L4 proxy cannot, and what does that extra capability cost?
- [ ] Control plane vs. data plane in a mesh: what does each do, and what still works if the control plane is down?
- [ ] What is a sidecar proxy, how does traffic get redirected through it, and what does it add per pod?
- [ ] What does mTLS prove that one-way TLS does not, and how does it relate to authorisation?
- [ ] What belongs in a gateway (authentication, rate limiting, routing), and what should stay in the service?
- [ ] What is a circuit breaker, and how is it different from a retry and from a timeout?

## B. Working depth
- [ ] NGINX vs. Envoy: how do static configuration and reloads compare with dynamic configuration through xDS? *(verify)*
- [ ] In Envoy, what are listeners, routes, clusters and endpoints, and how does a request move through them?
- [ ] Istio vs. Linkerd: how do they differ in proxy, scope and operational complexity? *(verify)*
- [ ] How does Istio ambient mode split L4 and L7 between ztunnel and waypoint proxies, and what does it change compared with sidecars? *(verify)*
- [ ] How does a mesh issue and rotate workload certificates, and what identity do they carry (SPIFFE)? *(verify)*
- [ ] How do you configure retries safely: retry budgets, idempotency, per-try timeouts and which status codes to retry?
- [ ] Outlier detection vs. active health checks: what does each catch, and what happens if too many hosts are ejected?
- [ ] How does weighted traffic splitting work for a canary, and how do header-based routing and traffic mirroring differ from it?
- [ ] Kong-style gateways: how do plugins, consumers and routes work, and what is the difference between database-backed and declarative mode? *(verify)*
- [ ] Gateway rate limiting: local vs. global limits, per-consumer quotas, and what happens when the limit store is unavailable?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] When is a mesh overkill? Name the team size, service count and requirements that would change your answer.
- [ ] How do retries at the client, the sidecar and the gateway multiply into a retry storm, and how do you prevent it?
- [ ] What are the latency, memory and CPU costs of sidecars at scale, and how would you measure them before committing? *(verify)*
- [ ] Mesh vs. libraries in the service (for example resilience4j or gRPC interceptors): what does each do better, and what about polyglot estates?
- [ ] Why does L4 load balancing fail for gRPC or HTTP/2, and how does an L7 proxy or client-side balancing fix it?
- [ ] How do you run a mesh upgrade safely: canary control planes, revision tags and data plane version skew? *(verify)*
- [ ] Sidecar startup and shutdown ordering: what breaks for jobs and init containers, and how do native sidecar containers help? *(verify)*
- [ ] What is the blast radius of a bad mesh or gateway configuration push, and what validation and staged rollout limit it?
- [ ] Does mTLS replace network policies and application authorisation? Argue where each layer belongs for zero-trust and for compliance.
- [ ] Managed gateways (AWS API Gateway, Apigee) vs. self-run Kong or Envoy: cost per request, limits, lock-in and operational load. *(verify)*

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] After enabling a mesh, some services see sporadic 503s during deploys. How do you find the cause and fix it?
- [ ] Clients get 504s from the gateway, but the service logs show success. Walk through what you check, hop by hop.
- [ ] Design an API gateway layer for a multi-tenant SaaS with API keys, per-tenant quotas, OAuth and versioned routes.
- [ ] Plan a mesh rollout across 50 existing services without downtime, including permissive mTLS and the path to strict mode.
- [ ] A customer requires all internal traffic to be encrypted, but you have ten services and one platform engineer. What do you propose?
- [ ] Design a canary release with automatic rollback based on error rate and latency from proxy metrics.
- [ ] One slow downstream dependency is taking out everything that calls it. Which timeouts, circuit breakers and bulkheads would you set, and where?

## E. Hands-on checks (things to be able to DO)
- [ ] Configure NGINX as a reverse proxy with upstream health handling, timeouts and a rate limit, and explain each directive.
- [ ] Write a minimal Envoy configuration that routes by path to two clusters, and inspect it through the admin endpoint.
- [ ] Install Istio or Linkerd on a local cluster, enable strict mTLS for one namespace and prove plaintext calls are rejected.
- [ ] Set up a 90/10 traffic split between two versions and verify the ratio from metrics.
- [ ] Inject faults (delay and abort) to test a retry and circuit breaking policy, and show the behaviour before and after.
- [ ] Debug a routing issue with `istioctl proxy-config` or the Envoy config dump and access logs. *(verify)*

---

**Good answers show:**
- Starting from the requirement (identity, traffic control, observability) and only then choosing the tool.
- A clear account of where each policy lives, so timeouts and retries do not fight one another across layers.
- Failure-mode thinking: control plane outages, bad configuration pushes, retry storms and certificate expiry.
- Honest cost accounting in latency, compute and the people needed to run the platform.
- Willingness to say a mesh is overkill, and to offer simpler options such as a gateway plus good client libraries.
- Practical debugging, reading proxy access logs, response flags and configuration dumps rather than guessing.

**Prove it:** Run three small services on a local kind cluster behind an Envoy-based gateway, then add a mesh. Implement strict mTLS, a canary split with automatic rollback, a retry budget and a circuit breaker, and use fault injection to show each one working. Measure the added latency and memory against the same setup with no mesh, and finish with a one-page recommendation on when the mesh is worth keeping.
