# 40 · Observability: logs, metrics, traces, alerts

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [39](../../phase-4-architecture/39-consensus-leader-election-locks/README.md) · ⏭️ [41 · Security basics for system design](../../phase-4-architecture/41-security-basics/README.md)

## The problem
"Orders are failing for some users." 40 services. Which one? Since when? Engineers guess for 2 hours. With good observability it takes 2 minutes.

## In one line
**Metrics say *that* something is wrong, traces say *where*, logs say *why* — and good alerts wake humans only when users are affected.**

☕ **Darshini:** **metrics** = hourly cash total (sales dropped!) · **trace** = following one order slip from counter to kitchen to table with timestamps · **logs** = the cook's diary ("12:31 gas ran out").

## Metrics
Numbers over time — dashboards & alerts.
- **RED** for services: Rate, Errors, Duration
- **USE** for resources: Utilisation, Saturation, Errors
- Business metrics: orders/min, payment success %
- ⚠️ don't put user IDs as metric labels (cardinality explosion)
Tools: Prometheus + Grafana, Datadog.

## Logs
**Structured** (JSON) with `request_id`, centralised, no secrets/PII. Tools: ELK/OpenSearch, Loki.

## Traces
One request across services: a **trace** made of **spans**; the **trace ID** is propagated in headers (and Kafka messages). The waterfall shows the slow hop. Standard: **OpenTelemetry**; tools: Jaeger, Tempo.

## SLI · SLO · SLA · error budget
- **SLI:** what you measure (% checkouts < 500 ms)
- **SLO:** your target (99.9% over 30 days)
- **SLA:** contract with penalties
- **Error budget:** 0.1% may fail → spend it on shipping, or freeze for reliability

## Good alerts
Alert on **user-facing symptoms**, actionable, with a runbook, low noise. Avoid alert fatigue.

## 🎯 Interview Line
**Q: How do you debug a slow request across microservices?**
> With distributed tracing: a trace ID is created at the edge and propagated through every service and message, so the trace waterfall shows which span is slow. Then I filter structured logs by that trace ID to see why, and use metrics dashboards to see how widespread the problem is.

## Key words
`Metrics` · `Logs` · `Traces` · `Span` · `Trace ID` · `RED` · `USE` · `SLI/SLO/SLA` · `Error budget` · `OpenTelemetry`
