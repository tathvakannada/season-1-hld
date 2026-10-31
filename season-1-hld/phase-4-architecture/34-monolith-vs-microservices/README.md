# 34 · Monolith vs Microservices

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [33](../../phase-3-communication/33-phase-3-recap/README.md) · ⏭️ [35 · Microservice patterns: discovery, timeouts, retries, circuit breaker, bulkhead](../../phase-4-architecture/35-microservice-resilience-patterns/README.md)

## The problem
200 engineers, one codebase: a one-line coupon change needs a full scary deploy; a PDF-invoice memory leak crashes order placement; search and payments can't scale separately.

## In one line
**A monolith is one deployable app; microservices are small, independently deployable services that own their data — team independence at the cost of distributed complexity.**

☕ **Darshini:** **one big kitchen** cooking everything vs **separate counters** (dosa, juice, chats) with their own staff and stock — if the juice machine breaks, dosas still come out.

## Monolith
One codebase, one build, one deploy (often one DB).
- ✅ simple to build, test, debug, deploy · fast in-process calls · easy transactions
- ❌ grows slow & risky · one bug crashes all · can't scale parts separately · teams collide
> 💡 **Modular monolith** (strict internal modules) — often the best start.

## Microservices
Each service: **one business capability**, **its own data**, **deployed independently**, talks via APIs & events.
- ✅ team ownership · deploy many times a day · scale hot parts · fault isolation
- ❌ network failures & latency · distributed transactions · tracing needed · more infrastructure

| | Monolith | Microservices |
|---|---|---|
| Deploy | all together | independently |
| Scale | whole app | per service |
| Data | one DB | DB per service |
| Debugging | easy | needs tracing |
| Best for | small teams, early | many teams, large scale |

## Splitting well
- Split by **business capability** (orders, payments), not technical layer
- **Strangler fig:** gateway in front, move one feature at a time
- Avoid the **distributed monolith** (shared DB, lockstep deploys, chatty sync chains)
- Conway's law: system structure mirrors team structure

## 🎯 Interview Line
**Q: Monolith or microservices — when would you choose each?**
> A monolith, ideally modular, for small teams and early products because it's simpler to build and operate. Microservices when many teams need to deploy independently and parts of the system need to scale separately — and the organisation can handle the cost of network failures, distributed data and observability.

## Key words
`Monolith` · `Modular monolith` · `Microservice` · `Bounded context` · `Strangler fig` · `Distributed monolith` · `Conway's law`
