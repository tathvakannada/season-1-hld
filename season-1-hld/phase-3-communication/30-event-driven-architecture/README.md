# 30 · Event-driven architecture (event sourcing, CQRS, outbox)

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [29](../../phase-3-communication/29-kafka/README.md) · ⏭️ [31 · Real-time: polling, long polling, WebSockets, SSE](../../phase-3-communication/31-real-time-communication/README.md)

## The problem
Services call each other in a tangle; every feature touches many services. And the business asks "how long did orders wait for restaurant acceptance last month?" — we only stored the *current* status.

## In one line
**Services publish facts about what happened and react to others' facts; advanced forms store the events themselves (event sourcing) and split write/read models (CQRS).**

☕ **Darshini:** the kitchen keeps a **diary of every action** (order taken, dosa poured, served); the board outside only shows "Ready: 41, 42" — built from the diary.

## Events vs commands
- **Command:** a request, may be rejected (`PlaceOrder`)
- **Event:** an immutable fact, past tense (`OrderPlaced`)

## Four flavours
1. **Event notification** — "order 1023 placed" (subscribers fetch details)
2. **Event-carried state** — the event contains the data (no call-backs)
3. **Event sourcing** — the event list **is** the source of truth
4. **CQRS** — separate write model and read models

## Event sourcing = bank passbook 📒
Store `OrderPlaced → PaymentConfirmed → RestaurantAccepted → RiderAssigned → Delivered`.
Current state = replay events (snapshots speed it up).
- ✅ full history & audit, answer new questions, rebuild views
- ❌ complexity, event schema evolution, eventual reads

## CQRS
- **Write side:** validates commands, enforces rules
- **Read side:** projections built from events, shaped for each screen
- ✅ fast tailored reads ❌ eventually consistent, more parts

## Choreography vs orchestration
Services react to each other's events vs a central coordinator tells them what to do.

## Outbox pattern (reliable publishing)
Problem: update DB **and** publish event → crash in between → they disagree.
Fix: write the change **and** an `outbox` row in **one DB transaction**; a relay publishes outbox rows to Kafka. (Or CDC from the DB log.)

## 🎯 Interview Line
**Q: How do you reliably update a database and publish an event?**
> With the transactional outbox pattern: in the same database transaction we write the change and an outbox row, and a separate relay publishes outbox rows to the message broker, retrying until it succeeds. Consumers are idempotent because an event may be delivered more than once. Change data capture is an alternative.

## Key words
`Event` · `Command` · `Event sourcing` · `Snapshot` · `CQRS` · `Projection` · `Choreography` · `Orchestration` · `Outbox`
