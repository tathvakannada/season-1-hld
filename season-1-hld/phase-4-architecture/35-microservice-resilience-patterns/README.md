# 35 · Microservice patterns: discovery, timeouts, retries, circuit breaker, bulkhead

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [34](../../phase-4-architecture/34-monolith-vs-microservices/README.md) · ⏭️ [36 · Rate Limiting](../../phase-4-architecture/36-rate-limiting/README.md)

## The problem
The coupon service slows down. Checkout waits 30 s per call, its threads fill up, the gateway runs out of connections — **all of Oota is down because of coupons** (cascading failure).

## In one line
**Services find each other dynamically and protect themselves from sick dependencies with timeouts, smart retries, circuit breakers and bulkheads.**

☕ **Darshini:** juice machine broken → put up a board **"Juice not available"** (open circuit) instead of making everyone wait; try the machine again every 15 minutes (half-open). Separate gas lines per counter = bulkhead.

## Service discovery
Instances register in a **registry** (Consul, etcd, Kubernetes DNS); callers look up healthy instances by name. A **service mesh** (Istio, Linkerd) adds retries, timeouts, mTLS via sidecars.

## Timeouts — always
Based on the dependency's p99 + margin (not 30 s). Pass the remaining deadline downstream.

## Retries — carefully
Only transient errors, only idempotent operations. **Exponential backoff + jitter** (100 ms, 200 ms, 400 ms + randomness). Limit attempts; retries at every layer multiply load.

## Circuit breaker
| State | Behaviour |
|---|---|
| **Closed** (normal) | calls flow, failures counted |
| **Open** (broken) | fail fast / fallback, no calls |
| **Half-open** (testing) | a few trial calls decide |
⚠️ Closed = good, Open = broken (opposite of a door!)

## Bulkhead
Separate thread/connection pools per dependency so one slow service can only exhaust its own pool.

## The coupon disaster, fixed
timeout 300 ms + 1 retry with jitter + circuit breaker + bulkhead + fallback "no coupon right now" → **coupons degrade, orders keep flowing**.

## 🎯 Interview Line
**Q: What is a cascading failure and how do you prevent it?**
> A slow or failing dependency exhausts its callers' threads and connections, so failure spreads upstream. Prevent it with timeouts on every call, limited retries with backoff and jitter, circuit breakers that fail fast, bulkheads that isolate resource pools, plus load shedding and graceful degradation.

## Key words
`Service discovery` · `Service mesh` · `Timeout` · `Exponential backoff` · `Jitter` · `Circuit breaker` · `Bulkhead` · `Cascading failure`
