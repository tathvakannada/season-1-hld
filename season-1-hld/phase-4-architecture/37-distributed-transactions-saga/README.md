# 37 · Distributed transactions: 2PC and Saga

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [36](../../phase-4-architecture/36-rate-limiting/README.md) · ⏭️ [38 · Unique ID generation at scale](../../phase-4-architecture/38-unique-id-generation/README.md)

## The problem
An order touches 3 services with 3 databases: payment charged ✅, restaurant service crashed ❌, no rider found ❌. Refund? Retry? There's no single database transaction anymore.

## In one line
**Across services, use a Saga: a sequence of local transactions, each with a compensating action to undo it if a later step fails.**

☕ **Darshini:** wedding catering: book the hall, the cooks, the decorators. Decorators cancel → you **cancel the cooks and get the hall refund**. You can't roll back time — you compensate.

## Two-phase commit (2PC)
Coordinator: 1) **prepare** (everyone votes yes/no, holding locks) 2) **commit/abort**.
- ✅ atomic, strong consistency
- ❌ **blocking** if the coordinator dies; locks across the network; slow; rarely used between microservices

## Saga
Local transactions step by step; on failure, run **compensations** backwards.
```
T1 charge payment      → C1 refund
T2 reserve restaurant  → C2 cancel reservation
T3 assign rider        (fails) → run C2, then C1
```

## Choreography vs orchestration
| Choreography | Orchestration |
|---|---|
| services react to events | a saga orchestrator sends commands |
| ✅ decoupled ❌ flow hard to see | ✅ clear flow & timeouts ❌ central logic |
Tools for orchestration: Temporal, AWS Step Functions, Camunda.

## Saga rules
- Every step & compensation **idempotent**
- Irreversible actions (SMS) go **last**
- Weak isolation → statuses like `PENDING`
- **Authorise payment first, capture last**
- Reliable messaging with the **outbox** pattern

## 🎯 Interview Line
**Q: How do you keep data consistent across microservices for an order?**
> With a saga: each service does a local transaction and publishes an event or reply, and if a later step fails we run compensating actions in reverse — for example cancel the restaurant reservation and refund the payment. Steps are idempotent, messages go through an outbox, and an orchestrator handles timeouts.

## Key words
`2PC` · `Coordinator` · `Saga` · `Compensating transaction` · `Choreography` · `Orchestration` · `Authorise vs capture`
