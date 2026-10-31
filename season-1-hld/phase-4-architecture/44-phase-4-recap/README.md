# 44 · Phase 4 recap: Oota as a real company

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [43](../../phase-4-architecture/43-deployments-and-infrastructure/README.md) · ⏭️ [45 · Back-of-the-envelope estimation](../../phase-5-interview-mode/45-back-of-the-envelope-estimation/README.md)

## The problem
Put Phase 4 together: what does a big company's architecture look like, and why does each layer exist?

## In one line
**Microservices with ownership, protected by resilience patterns and rate limits, coordinated by sagas and events, observable, secure, and shipped safely many times a day.**

☕ **Darshini:** the darshini is now a **chain**: counters with their own staff, guards at the gate, a manager elected per shift, CCTV everywhere, and new recipes tested on a few customers first.

## Phase 4 in 10 problems
| Problem | Tool |
|---|---|
| one scary codebase | microservices / modular monolith |
| coupon slowness killed checkout | timeouts, retries + jitter, circuit breaker, bulkhead |
| bots hammering APIs | rate limiting |
| payment done, restaurant crashed | Saga + compensations |
| duplicate IDs across shards | Snowflake / UUID v7 |
| job ran 3×, split brain | consensus, leader election, fencing |
| "orders failing — where?" | metrics, traces, logs, SLOs |
| data leaks, IDOR | authN/authZ, OAuth/OIDC, secrets, OWASP |
| unique counts, nearby, autocomplete | Bloom, HLL, geohash, trie |
| scary deploys | containers, Kubernetes, CI/CD, canary |

## Oota v4 (layers)
- **Edge:** CDN/WAF → API gateway (auth, rate limits)
- **Services:** on Kubernetes, service mesh (mTLS, timeouts, retries, breakers)
- **Async backbone:** Kafka events + job queues + outbox
- **Data:** Postgres (sharded) · Redis · MongoDB · Cassandra · Elasticsearch · S3
- **Platform:** etcd/ZooKeeper · OpenTelemetry · Vault · CI/CD with canaries

## 🎯 Interview Line
**Q: What do you add when a startup's system becomes a large company's platform?**
> Clear service ownership with independent deploys, resilience patterns between services, rate limiting at the edge, sagas and events for cross-service workflows, scalable IDs, coordination for leaders and locks, full observability with SLOs, security at every layer, and automated CI/CD with canary releases.

## Key words
`Phase 4` · `Oota v4` · `Resilience` · `Saga` · `Observability` · `Canary`
