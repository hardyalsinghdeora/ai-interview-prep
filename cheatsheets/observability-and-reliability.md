# Observability and Reliability Cheatsheet

Covers metrics, logs and traces, OpenTelemetry, service level terms, monitoring methods, incident response, resilience testing and disaster recovery.

> Related: [Observability and reliability skill](../skills/observability-and-reliability.md) · [Debugging and troubleshooting](../topics/12-debugging-troubleshooting/README.md) · [Cloud, DevOps and integration](../topics/08-cloud-devops-integration/README.md) · [containers-and-kubernetes.md](containers-and-kubernetes.md) · [ci-cd-and-iac.md](ci-cd-and-iac.md) · [distributed-systems.md](distributed-systems.md) · [Monitoring tools cheatsheet](monitoring-tools.md)

## Three pillars

| Term | What it means |
|---|---|
| **Observability** | How well you can understand a system's internal state from its outputs, including failures nobody predicted. |
| **Monitoring** | Tracking known signals and alerting on them, which answers whether something predefined has gone wrong. |
| **Metric** | A numeric measurement over time, cheap to store and ideal for dashboards and alerts. |
| **Log** | A timestamped record of an event, rich in detail but costly at high volume. |
| **Trace** | The path of one request across services, which shows where time went and what failed. |
| **Span** | One timed operation within a trace, with attributes and a link to its parent. |
| **Structured logging** | Logs written as fields such as JSON, so they can be searched and aggregated reliably. |
| **Correlation ID** (correlation identifier) | An identifier passed along a request, which ties logs from different services together. |
| **Context propagation** | Passing trace identifiers across service boundaries so spans join into one trace. |
| **Sampling** | Keeping only some traces or logs to control cost; head sampling decides early, tail sampling after completion. |

## Metric types and cardinality

| Term | What it means |
|---|---|
| **Counter** | A value that only rises, such as total requests; alert on its rate rather than its raw value. |
| **Gauge** | A value that can rise and fall, such as memory in use or queue depth. |
| **Histogram** | Counts observations in buckets, allowing percentiles to be estimated across many instances. |
| **Summary** | Client-calculated quantiles; these generally cannot be aggregated meaningfully across instances. |
| **Label** | A dimension on a metric, such as `status` or `region`; each unique combination creates a new time series. |
| **Cardinality** | The number of unique label combinations; high values, such as per-user labels, drive up cost and slow queries. |
| **Time series** | A sequence of timestamped values for one metric and label set, the basic storage unit. |
| **Prometheus** | An open-source metrics system that scrapes targets by pull and queries them with PromQL. |
| **PromQL** (Prometheus Query Language) | The language for querying and aggregating Prometheus metrics, for example `rate()` over a window. |
| **Grafana** | An open-source dashboarding tool that visualises metrics, logs and traces from many data sources. |

## OpenTelemetry

| Term | What it means |
|---|---|
| **OTel** (OpenTelemetry) | A vendor-neutral CNCF standard and toolset for producing telemetry, reducing lock-in to any monitoring vendor. |
| **OTLP** (OpenTelemetry Protocol) | The standard protocol for sending traces, metrics and logs to a backend. |
| **OTel Collector** (OpenTelemetry Collector) | A service that receives, processes and exports telemetry, keeping the pipeline separate from applications. |
| **Instrumentation** | Code or an agent that emits telemetry; auto-instrumentation gives quick coverage with few code changes. |
| **Semantic conventions** | Standard attribute names such as `http.request.method`, making telemetry consistent across services. |

## SLI, SLO, SLA

| Term | What it means |
|---|---|
| **SLI** (Service Level Indicator) | A measured aspect of service health, such as the share of successful requests. |
| **SLO** (Service Level Objective) | An internal target for an SLI over a time window, such as 99.9% over 30 days. |
| **SLA** (Service Level Agreement) | A contractual promise with consequences such as credits; it should be looser than the SLO. |
| **Error budget** | The unreliability an SLO allows (100% minus target), which balances feature speed against reliability work. |
| **Error budget policy** | The agreed response when budget runs out, such as pausing risky releases. |
| **Burn rate** | How fast the error budget is being spent relative to plan; a rate of 1 exhausts it exactly on time. |
| **Multi-window burn-rate alert** | Alerts when both a long and a short window burn quickly, catching real problems with fewer false pages. |
| **Toil** | Manual, repetitive, automatable work that scales with the service, which SRE teams aim to reduce. |
| **SRE** (Site Reliability Engineering) | Applying software engineering to operations, using SLOs and automation to run reliable services. |

| Availability | Downtime per 30-day month | Downtime per year |
|---|---|---|
| 99% | 7.2 hours | 3.65 days |
| 99.9% | 43.2 minutes | 8.76 hours |
| 99.95% | 21.6 minutes | 4.38 hours |
| 99.99% | 4.32 minutes | 52.6 minutes |

## Monitoring methods and latency

| Term | What it means |
|---|---|
| **RED method** (Rate, Errors, Duration) | Per-service monitoring from the user's view, suiting request-driven services. |
| **USE method** (Utilisation, Saturation, Errors) | Per-resource monitoring for CPU, memory, disk and network, which suits finding infrastructure bottlenecks. |
| **Four golden signals** | Latency, traffic, errors and saturation, the core service signals from Google's SRE guidance. |
| **Saturation** | How full a resource is, often shown by queueing; it tends to predict trouble before errors appear. |
| **Percentile** | The value below which a given share of samples falls, showing the spread that averages hide. |
| **p50** (50th percentile) | The median: half of requests are faster, showing the typical experience. |
| **p95 and p99** (95th and 99th percentiles) | Slow-end latency; these reveal tail problems that heavy users often hit. |
| **Tail latency** | The slowest portion of requests, which matters because fan-out calls often wait for the slowest. |
| **Averaging percentiles** | A common mistake: percentiles cannot be averaged across instances, so aggregate histograms instead. |
| **Symptom-based alerting** | Paging on user-visible impact rather than causes, which reduces noise and pages that need no action. |
| **Alert fatigue** | Too many low-value alerts train responders to ignore them, so real incidents get missed. |

## Incidents

| Term | What it means |
|---|---|
| **Incident** | An unplanned event that disrupts or threatens a service, handled through a defined response process. |
| **Severity level** | A rating of impact, such as SEV1 to SEV4, which sets urgency and who gets paged. |
| **IC** (Incident Commander) | Coordinates the response, makes decisions and delegates, rather than doing hands-on debugging. |
| **Communications lead** | Keeps stakeholders and status pages updated, freeing responders to focus on the fix. |
| **Operations lead** | Directs the technical investigation and mitigation work during an incident. |
| **Scribe** | Records the timeline and decisions, which makes the later review far more accurate. |
| **On-call** | The rota of engineers responsible for responding to alerts; its load is a sustainability concern. |
| **Runbook** | Step-by-step guidance for a known alert or task, which speeds up response under pressure. |
| **Mitigation** | Reducing user impact quickly, such as rolling back, before the root cause is fully understood. |
| **RCA** (Root Cause Analysis) | Working out why an incident happened, usually finding several contributing factors rather than one cause. |
| **Five whys** | Repeatedly asking why to get past symptoms, though it can oversimplify complex failures. |
| **Blameless postmortem** | A written incident review focused on systems rather than individuals, so people report problems honestly. |
| **MTTR** (Mean Time To Recovery) | Average time to restore service; the R can also mean repair, respond or resolve, so define it. |
| **MTTD** (Mean Time To Detect) | Average time from failure starting to being noticed, showing the quality of monitoring. |
| **MTBF** (Mean Time Between Failures) | Average working time between failures, a measure of how often things break. |

## Resilience and chaos engineering

| Term | What it means |
|---|---|
| **Chaos engineering** | Deliberately injecting failure under controlled conditions to test assumptions and find weaknesses before users do. |
| **Steady-state hypothesis** | The expected normal behaviour, defined up front, that a chaos experiment checks still holds. |
| **Blast radius** | How much can be affected by a failure or experiment, which should start small. |
| **Game day** | A planned exercise simulating failure, testing both systems and team response. |
| **Fault injection** | Introducing errors, latency or outages deliberately to see how a system copes. |
| **Circuit breaker** | Stops calling a failing dependency for a while, preventing cascading failure and allowing recovery. |
| **Bulkhead** | Isolates resources per dependency or workload, so one failure cannot use up everything. |
| **Graceful degradation** | Serving a reduced experience when something fails rather than failing completely. |
| **Load shedding** | Deliberately rejecting some requests under overload to keep the rest working. |
| **Retry with backoff and jitter** | Retrying after growing, randomised delays, which avoids synchronised retry storms. |

## Disaster recovery

| Term | What it means |
|---|---|
| **DR** (Disaster Recovery) | Plans and systems for restoring service after a major failure such as losing a region. |
| **RTO** (Recovery Time Objective) | The longest acceptable time to restore service after a disaster. |
| **RPO** (Recovery Point Objective) | The largest acceptable data loss, measured in time before the failure. |
| **Backup and restore** | The cheapest DR approach: restore from backups, accepting the longest recovery time. |
| **Pilot light** | Core data and minimal services stay running elsewhere, then the rest is started during failover. |
| **Warm standby** | A scaled-down but working copy runs elsewhere and scales up on failover, cutting recovery time. |
| **Active-active** | Multiple sites serve traffic at once, offering the quickest recovery but the highest cost and complexity. |
| **Failover** | Moving traffic to a standby system when the primary fails, which must be rehearsed to be trusted. |
| **Restore test** | Regularly proving backups can actually be restored, because an untested backup may not work. |

| DR strategy | Relative cost | Typical RTO and RPO |
|---|---|---|
| Backup and restore | Lowest | Hours or more |
| Pilot light | Low | Tens of minutes to hours |
| Warm standby | Medium to high | Minutes |
| Active-active | Highest | Near zero, depending on replication |
