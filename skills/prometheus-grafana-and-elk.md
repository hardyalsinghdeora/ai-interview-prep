# Prometheus, Grafana & ELK

Knowing what an SLO is does not help when Prometheus is being OOM-killed or logs have stopped reaching Kibana, so interviewers test whether you can actually operate the tools: write the query, fix the scrape, tame the alert and explain the bill. Platform, SRE, Staff backend and Engineering Manager roles ask most, usually by describing a noisy or expensive monitoring stack and asking what you would change.

> Related: [Observability and reliability](observability-and-reliability.md) · [Elasticsearch & OpenSearch](elasticsearch-and-opensearch.md) · [Kubernetes](kubernetes.md) · [Debugging & troubleshooting](../topics/12-debugging-troubleshooting/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Choose a Prometheus metric type for request count, queue depth and request latency. Why can histogram buckets from several pods be aggregated when summary quantiles cannot?
- [ ] Why does Prometheus pull metrics rather than receive pushes, and when is the Pushgateway appropriate rather than a mistake?
- [ ] `rate()` vs. `irate()` vs. `increase()`: what does each return, and how do they handle a counter reset?
- [ ] Prometheus vs. Alertmanager: which one evaluates rules, and which handles grouping, inhibition, silences and routing?
- [ ] What does Grafana store and what does it only query? Explain data sources, dashboards and template variables.
- [ ] What job do Elasticsearch, Logstash, Kibana and Beats each do in a log pipeline, and when can you leave Logstash out?
- [ ] Where do Datadog, New Relic and Splunk each come from, what is each strongest at, and how do you query data in each? *(verify)*
- [ ] Jaeger, Sentry and PagerDuty solve different problems. What does each tell you or do during an incident, and where do they connect?

## B. Working depth
- [ ] Write p95 latency per route from a histogram. Why do you need `rate` and `sum by (le, route)`, and how do bucket boundaries limit the accuracy of the answer?
- [ ] Alert rules: how do `for`, `keep_firing_for` and `absent()` deal with flapping and missing series, and what does `up == 0` miss when a target vanishes from discovery? *(verify)*
- [ ] Kubernetes scraping: how do `kubernetes_sd_configs` and the Prometheus Operator's ServiceMonitor discover targets, and when do `relabel_configs` and `metric_relabel_configs` each run? *(verify)*
- [ ] PromQL joins: how do `on`, `ignoring` and `group_left` let you add a team label from an info-style metric onto an error rate?
- [ ] Alertmanager: what do `group_by`, `group_wait`, `group_interval` and `repeat_interval` each control, and how would you write an inhibit rule that mutes pod alerts during a node failure?
- [ ] Grafana as code: compare file provisioning, Terraform and a dashboard library. How do you keep template variables from overloading a data source, and how do you stop dashboard sprawl? *(verify)*
- [ ] Logstash: what do the input, filter and output stages do, when do you pick `grok` over `dissect`, and what do persistent queues and the dead letter queue each protect against?
- [ ] Filebeat: how does its registry give at-least-once delivery, and how do you handle multiline stack traces, log rotation and Kubernetes autodiscovery? *(verify)*
- [ ] Log indices: `text` vs. `keyword` for fields such as `status` and `message`, and how data streams, rollover and ILM move data through hot, warm, cold and delete. *(verify)*
- [ ] Datadog: how do tags relate to custom metric billing, and how would you stop a team's new tag from multiplying the cost? *(verify)*
- [ ] Splunk and New Relic: how do forwarders, indexers and search heads fit together, and how would you express "top ten errors in the last hour" in SPL and in NRQL? *(verify)*
- [ ] Sentry: how do fingerprints, releases and source maps change what you see, and how do you sample and scrub events on a high-traffic front end? *(verify)*
- [ ] Jaeger: how do the collector, storage backend and query service fit together, how do applications send spans using OTLP, and which storage would you choose? *(verify)*
- [ ] PagerDuty: how do services, escalation policies, schedules and urgency interact, and how does `dedup_key` decide whether an event opens a new incident? *(verify)*

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] How do you find which metric and label caused a cardinality jump, and which controls would you put in place (`sample_limit`, dropping labels, per-team limits, CI checks)? *(verify)*
- [ ] Where does one Prometheus server stop being enough? Compare HA pairs, federation, remote write and long-term stores such as Thanos, Mimir and VictoriaMetrics. *(verify)*
- [ ] Native vs. classic histograms: what problem do native histograms solve, what do they cost, and what would hold you back from migrating? *(verify)*
- [ ] With two identical Prometheus servers, how are duplicate alerts avoided? Why should each one send to every Alertmanager rather than to a load balancer in front of them?
- [ ] Elasticsearch for logs: how do you choose shard size and count, what causes a mapping explosion, and what makes oversharding so expensive at high index counts? *(verify)*
- [ ] Would you put Kafka between Beats and Logstash? Describe what happens to the pipeline when Elasticsearch slows down, with and without that buffer.
- [ ] Self-run Prometheus, Grafana and ELK vs. Datadog, New Relic or Splunk: compare pricing models, staffing and lock-in, and say how much of that lock-in OpenTelemetry really removes. *(verify)*
- [ ] How do the licences of Elasticsearch, OpenSearch, Grafana and Sentry affect what a company can self-host or offer to its own customers? *(verify)*
- [ ] Logs, traces and error events routinely capture personal data. How do you control redaction, retention and access across these tools to meet GDPR-style obligations?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Prometheus starts getting OOM-killed an hour after a deploy. Walk through how you confirm the cause, restore monitoring and stop it recurring.
- [ ] One database failure produced 200 pages in ten minutes. Redesign the labels, routing, grouping and inhibition from Alertmanager through to PagerDuty.
- [ ] A Grafana dashboard takes 40 seconds to load and visibly loads Prometheus. How do you diagnose it, and where do recording rules, query ranges and variables help?
- [ ] One service's logs have stopped appearing in Kibana while the application still writes them. Trace the path from the container to the index and name what you check at each hop.
- [ ] A customer reports failing checkouts. Walk through the move from PagerDuty page to Grafana, Jaeger, Kibana and Sentry, and what each must carry (trace ID, release, tenant) to make that possible.
- [ ] Design metrics and logging for 200 services across three clusters with 13-month metric retention and per-team cost attribution. Which tools would you run, and which would you buy?
- [ ] Finance wants the Datadog or Splunk bill cut by 40% and suggests moving to Prometheus, Grafana and ELK. How do you assess that, what would you try first, and how would you migrate without an alerting gap?

## E. Hands-on checks (things to be able to DO)
- [ ] Run Prometheus, Alertmanager and Grafana in Docker Compose, scrape an instrumented service, and use relabelling to drop one high-cardinality label.
- [ ] Write PromQL for a 5xx ratio per route, the ten largest metrics by series count, and disk-full prediction with `predict_linear`, then cover one rule with `promtool test rules`.
- [ ] Write an Alertmanager config with team routing and an inhibit rule, validate it with `amtool`, and deliver a test alert to a webhook stub or a PagerDuty trial service.
- [ ] Build a Grafana dashboard with template variables and links from a latency panel to traces, export it, and provision it from a repository.
- [ ] Ship logs through Filebeat and Logstash into Elasticsearch, parse them with `grok` or `dissect`, apply an ILM policy, and find a request by trace ID in Kibana. *(verify)*
- [ ] Send a trace to Jaeger and an error to Sentry from one failing request, upload source maps or debug symbols, and show the trace ID linking the two.

---

**Good answers show:**
- Fluency at the keyboard: writing the PromQL, Alertmanager config or log parser rather than describing it from a distance.
- Tracing a data path hop by hop (exporter, scrape, TSDB, rule, Alertmanager, pager) to find where signal was lost.
- Cardinality and ingest volume treated as design constraints, with limits and owners, before they become an outage or an invoice.
- Alert routing designed around who gets woken and why, with grouping and inhibition that reflect how failures really cascade.
- Build-versus-buy reasoning that counts staffing, migration and lock-in as well as the headline cost.
- Awareness that monitoring systems fail too: who watches Prometheus, what happens when the log pipeline backs up, and how sensitive data is kept out.

**Prove it:** Run two small services and a worker on a local cluster with Prometheus, Alertmanager, Grafana, an ELK pipeline, Jaeger and self-hosted or trial Sentry. Add one recording rule, one inhibit rule and an ILM policy, then introduce a high-cardinality label and a failing dependency on purpose. Write up how you found each problem, the before and after series counts, and a short comparison with the same setup on one commercial tool's free trial.
