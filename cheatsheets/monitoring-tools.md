# Monitoring Tools Cheatsheet

Covers the tools rather than the concepts: Prometheus, Grafana, the ELK stack, Datadog, New Relic, Splunk, Jaeger, Sentry and PagerDuty.

> Related: [Observability and reliability skill](../skills/observability-and-reliability.md) · [Elasticsearch and OpenSearch skill](../skills/elasticsearch-and-opensearch.md) · [Debugging and troubleshooting](../topics/12-debugging-troubleshooting/README.md) · [observability-and-reliability.md](observability-and-reliability.md) · [containers-and-kubernetes.md](containers-and-kubernetes.md) · [search-and-caching.md](search-and-caching.md) · [Prometheus, Grafana & ELK skill](../skills/prometheus-grafana-and-elk.md)

## Tools at a glance

| Tool | Main job | Hosting |
|---|---|---|
| Prometheus | Metrics collection and alert rules | Self-hosted, open source |
| Grafana | Dashboards over many data sources | Self-hosted or Grafana Cloud |
| ELK stack | Log ingestion, search and analysis | Self-hosted or Elastic Cloud |
| Datadog | All-in-one metrics, logs, traces and more | Software as a service |
| New Relic | All-in-one, strongest roots in application monitoring | Software as a service |
| Splunk | Machine data search, security analytics | Self-managed or cloud |
| Jaeger | Distributed tracing | Self-hosted, open source |
| Sentry | Application errors and code-level performance | Software as a service or self-hosted |
| PagerDuty | On-call, alert routing and incident response | Software as a service |

## Prometheus: collection

| Term | What it means |
|---|---|
| **Prometheus** | An open-source metrics system that scrapes targets and stores time series locally; the usual default for Kubernetes monitoring. |
| **Scrape** | Prometheus fetching metrics from a target over HTTP on a schedule; each attempt also records whether the target was reachable. |
| **Pull model** | Prometheus collects from targets rather than receiving pushes, so a dead target is detected as soon as a scrape fails. |
| **`job` and `instance` labels** | Added at scrape time: `job` names a group of targets and `instance` identifies one, so queries can slice by either. |
| **`scrape_interval`** | How often targets are scraped; shorter gives finer detail but raises load and storage. |
| **Service discovery** | Finds targets automatically from sources such as Kubernetes or EC2, so changing fleets need no manual configuration. |
| **Relabelling** | Rewrites or drops targets and labels through `relabel_configs`, which helps tidy data and control cardinality. |
| **Exporter** | A small process that translates a system's statistics into Prometheus format, for software that cannot expose metrics itself. |
| **Node Exporter** | The official exporter for host metrics such as CPU, memory, disk and network on Linux and Unix machines. |
| **cAdvisor** (Container Advisor) | Reports per-container resource usage; it is built into the Kubernetes kubelet, so clusters usually scrape it there. |
| **kube-state-metrics** | Exposes the state of Kubernetes objects, such as desired versus ready replicas, rather than resource usage. |
| **Blackbox Exporter** | Probes endpoints from outside over HTTP, TCP, DNS or ICMP, which tests what a client actually experiences. |
| **Pushgateway** | Holds metrics pushed by short-lived batch jobs so they can still be scraped; not a general push pipeline. |
| **ServiceMonitor** | A Prometheus Operator custom resource that declares which Kubernetes services to scrape, replacing hand-edited configuration. |

## Prometheus: storage and metric types

| Term | What it means |
|---|---|
| **Time series** | A stream of timestamped values identified by a metric name plus its labels. |
| **Label** | A key-value pair on a metric; every new combination creates a new series, so unbounded values are dangerous. |
| **TSDB** (Time Series Database) | Prometheus's local on-disk store; default retention is 15 days *(verify)*, so it is not long-term storage. |
| **Remote write** | Streams samples to external storage such as Thanos or Mimir for long retention and global views. |
| **Thanos** | Adds object-storage retention and a global query view across several Prometheus servers. |
| **Counter** | A value that only goes up, resetting on restart; use it through `rate()` for requests or errors. |
| **Gauge** | A value that can rise and fall, such as memory in use or queue depth. |
| **Histogram** | Counts observations in configured buckets, producing `_bucket`, `_sum` and `_count` series; percentiles are estimated at query time. |
| **Summary** | Calculates quantiles inside the client, so the results are precise per instance but cannot be meaningfully aggregated. |
| **Native histogram** | A newer histogram with automatic exponential buckets, giving better resolution at lower cost; check version support *(verify)*. |

| Histogram vs summary | Histogram | Summary |
|---|---|---|
| Quantile calculated | On the server, at query time | In the client, at observation time |
| Aggregate across instances | Yes | Not meaningfully |
| Accuracy depends on | Bucket boundaries | Client configuration |
| Change quantile later | Yes | No, requires a code change |
| Default choice | Usually this one | Single-instance, known quantiles |

## PromQL

| Term | What it means |
|---|---|
| **PromQL** (Prometheus Query Language) | The functional query language for selecting and aggregating time series in dashboards and alert rules. |
| **Instant vector** | One sample per series at a single moment; the form that alert rules evaluate. |
| **Range vector** | Samples over a time window, such as `[5m]`; the input that `rate()` needs. |
| **`rate()`** | Per-second average increase of a counter over a window, handling resets; the standard choice for alerts. |
| **`irate()`** | Per-second rate from the last two samples, which suits fast graphs but is too jumpy for alerts. |
| **`increase()`** | Total rise of a counter over a window; extrapolated, so results may not be whole numbers. |
| **`histogram_quantile()`** | Estimates a percentile from histogram bucket rates; accuracy depends on how well the buckets fit. |
| **`absent()`** | Returns a result when a series is missing, which catches exporters that silently stop reporting. |
| **Recording rule** | Precomputes an expensive query and stores it as a new series, which speeds up dashboards and alerts. |
| **Alerting rule** | A PromQL condition that sends alerts to Alertmanager when it returns results. |
| **`for` clause** | How long a condition must hold before an alert fires; moves it from pending to firing and reduces flapping. |

```promql
# Requests per second by service
sum by (service) (rate(http_requests_total[5m]))

# Error ratio
sum(rate(http_requests_total{status=~"5.."}[5m])) / sum(rate(http_requests_total[5m]))

# 95th percentile latency (rate first, then sum, keeping the le label)
histogram_quantile(0.95, sum by (le) (rate(http_request_duration_seconds_bucket[5m])))
```

## Alertmanager

| Term | What it means |
|---|---|
| **Alertmanager** | Receives alerts from Prometheus, then deduplicates, groups and routes them; decides who hears, while rules decide what fires. |
| **Route** | A tree of label matchers that sends each alert to the right receiver. |
| **Receiver** | A notification destination, such as Slack, email or PagerDuty. |
| **Grouping** | Combines related alerts into one notification, so an outage does not send hundreds of pages. |
| **Inhibition** | Mutes some alerts while another is firing, such as service alerts during a cluster outage. |
| **Silence** | A time-limited mute matched on labels, used for planned maintenance. |

## Grafana

| Term | What it means |
|---|---|
| **Grafana** | An open-source visualisation tool that queries existing data sources rather than storing data; licensed under AGPLv3 *(verify)*. |
| **Data source** | A connection to a backend such as Prometheus, Loki or a SQL database. |
| **Dashboard variable** | A dropdown that parameterises queries, so one dashboard can serve many services or environments. |
| **Annotation** | A marker for an event such as a deployment, which helps correlate changes with graphs. |
| **Grafana Alerting** | Built-in alert rules across data sources, with contact points and notification policies for routing. |
| **Provisioning** | Defines dashboards and data sources in files, so they can be version-controlled and reproduced. |
| **Loki** | A log store that indexes only labels, not full text, which makes it cheaper than full-text search engines. |
| **Tempo** | Grafana's trace store, designed to run mostly on object storage to keep costs low. |
| **Mimir** | A horizontally scalable, Prometheus-compatible store for long-term metrics. |

## ELK stack

| Term | What it means |
|---|---|
| **ELK stack** (Elasticsearch, Logstash, Kibana) | A popular log pipeline: ship and parse with Logstash, store in Elasticsearch, explore in Kibana. |
| **Elasticsearch** | The distributed search engine at the centre, storing logs in inverted indices for fast full-text queries. |
| **Logstash** | A pipeline of inputs, filters and outputs that parses and enriches logs; powerful but relatively resource-heavy. |
| **Kibana** | The web interface for searching logs and building dashboards over Elasticsearch data. |
| **Beats** | Lightweight data shippers, such as Filebeat for log files, that run close to the source. |
| **Grok** | A Logstash filter that uses named patterns to turn unstructured log lines into fields. |
| **ILM** (Index Lifecycle Management) | Moves indices through hot, warm, cold and delete phases by age, which controls storage cost. |
| **EFK stack** (Elasticsearch, Fluentd or Fluent Bit, Kibana) | Replaces Logstash with a lighter collector, a common arrangement on Kubernetes. |
| **OpenSearch** | The open-source fork created in 2021 after Elastic changed its licence; Elastic's licensing has since changed again *(verify)*. |

## Datadog and New Relic

| Term | What it means |
|---|---|
| **Datadog** | A commercial platform joining metrics, logs, traces and more in one interface; fast to adopt, but costs need active watching. |
| **Datadog Agent** | Software on each host or node that collects metrics, logs and traces and sends them to Datadog. |
| **DogStatsD** | The agent's StatsD-compatible server, which applications use to send custom metrics. |
| **Datadog tag** | A `key:value` label such as `env:prod`; unique tag combinations drive custom metric volume and cost *(verify)*. |
| **Logging without Limits** | Datadog's split between ingesting logs and indexing them, so only searchable logs carry index cost *(verify)*. |
| **RUM** (Real User Monitoring) | Captures performance and errors from real browser and mobile sessions. |
| **Synthetic monitoring** | Scripted checks from chosen locations, which catch outages even when no real users are active. |
| **New Relic** | A commercial observability platform with deep language agents; pricing is mainly based on data and users *(verify)*. |
| **NRQL** (New Relic Query Language) | A SQL-like language for querying all telemetry stored in New Relic. |
| **Apdex** (Application Performance Index) | A score from 0 to 1 based on a response-time threshold, summarising user satisfaction in one number. |

## Splunk

| Term | What it means |
|---|---|
| **Splunk** | A platform for searching and analysing machine data, strong in security and compliance; owned by Cisco since 2024. |
| **Forwarder** | An agent that sends data to indexers; the Universal Forwarder is the lightweight, common choice. |
| **Indexer** | Stores incoming data in time-based buckets and answers searches over it. |
| **Search head** | The component that takes user searches, distributes them to indexers and merges the results. |
| **SPL** (Search Processing Language) | Splunk's pipe-based language, where each command transforms the results of the previous one. |
| **Sourcetype** | Tells Splunk the format of incoming data, so timestamps and fields are parsed correctly. |
| **HEC** (HTTP Event Collector) | A token-authenticated HTTP endpoint for sending events directly, without a forwarder. |

```text
index=web status>=500 earliest=-1h | stats count by host | sort -count | head 5
```

## Jaeger

| Term | What it means |
|---|---|
| **Jaeger** | An open-source distributed tracing system, created at Uber, for following requests across services and locating latency. |
| **Jaeger Collector** | Receives spans from applications, processes them and writes them to storage. |
| **Jaeger storage backend** | Where spans persist, commonly Cassandra, Elasticsearch or OpenSearch; in-memory suits only local development. |
| **OTLP** (OpenTelemetry Protocol) | The vendor-neutral format Jaeger accepts natively, so instrumentation uses OpenTelemetry rather than Jaeger-specific clients. |
| **Remote sampling** | Lets a central configuration set sampling rates per service, avoiding application redeployments to change them. |
| **Jaeger v2** | The rearchitected version built on the OpenTelemetry Collector; the older per-host agent is retired *(verify)*. |

## Sentry

| Term | What it means |
|---|---|
| **Sentry** | Application error tracking with stack traces and code context; it answers which line broke, rather than which host is unhealthy. |
| **DSN** (Data Source Name) | The client key telling a Sentry SDK where to send events; visible in browser code by design. |
| **Sentry issue** | A group of similar error events, so one bug appears once with a count rather than thousands of times. |
| **Fingerprint** | The rule that decides how events group into issues; customise it when grouping is too broad or narrow. |
| **Breadcrumbs** | A trail of actions and logs before an error, which shows how the application reached that state. |
| **Source map** | Maps minified JavaScript back to original source, so stack traces are readable; usually uploaded in CI. |
| **Release tracking** | Ties errors to deployed versions, showing which release introduced or regressed a problem. |

## PagerDuty

| Term | What it means |
|---|---|
| **PagerDuty** | A commercial platform that turns alerts into pages for the right on-call engineer and tracks the response. |
| **PagerDuty service** | The thing being supported, which receives alerts and links to an escalation policy. |
| **Schedule** | The on-call rotation defining who is responsible at any given time. |
| **Escalation policy** | An ordered list of who to notify, and when to move on if nobody acknowledges. |
| **Incident states** | Triggered, acknowledged and resolved; acknowledging stops further escalation while someone investigates. |
| **Urgency** | High urgency notifies through the on-call's rules immediately, while low urgency avoids waking people for non-critical issues. |
| **Events API v2** | The interface monitoring tools use to trigger, acknowledge and resolve alerts programmatically. |
| **`dedup_key`** | Identifies an alert in the Events API, so repeated events update one alert rather than creating new ones. |
| **Event Orchestration** | Rules that route, enrich or suppress incoming events before they become incidents, which reduces noise. |
