# 33 · Phase 3 recap: Oota's order flow end to end

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [32](../../phase-3-communication/32-api-design-deep-dive/README.md) · ⏭️ [34 · Monolith vs Microservices](../../phase-4-architecture/34-monolith-vs-microservices/README.md)

## The problem
Put every Phase 3 tool together: what really happens when you tap "Place order"?

## In one line
**Do the minimum synchronously, then publish events that independent services handle asynchronously — reliably, idempotently, with live updates back to the user.**

☕ **Darshini:** token system end to end: cashier takes payment and gives a token (sync), kitchen slips go on the rail (queue), the "ready!" announcement (pub/sub), the CCTV (Kafka), the display board (SSE).

## The flow
1. App → `POST /v1/orders` (+ Idempotency-Key) → gateway → order service
2. **Sync:** validate cart & price, authorise payment (with timeout), then **one transaction**: save order + outbox row
3. Respond **"Order placed ✅"** (~300 ms)
4. Outbox relay → Kafka topic `orders` (key = order_id)
5. **Async, in parallel:** restaurant · rider matching · notifications (queue → workers, retries, DLQ) · analytics · loyalty
6. Status read model (CQRS) updates the order timeline
7. Customer app gets live updates via SSE/WebSocket
8. Payment gateway webhook → idempotent handler

## Phase 3 in one table
| Problem | Tool |
|---|---|
| 12-second checkout | sync vs async |
| spikes overwhelm SMS | message queue + DLQ |
| 5 services care about 1 event | pub/sub |
| huge event volume, replay | Kafka |
| tangled calls, need history | event-driven, event sourcing, CQRS, outbox |
| live map & status | SSE / WebSockets |
| double charges, big lists | idempotency keys, cursor pagination |

## Failure drills
- Restaurant service down 5 min → events wait in Kafka, it catches up
- Duplicate event → idempotent consumers
- Crash after DB commit → outbox still publishes
- Phone loses network → reconnect & resume

## 🎯 Interview Line
**Q: Walk me through what happens when a user places an order.**
> The order service synchronously validates the cart, authorises payment and, in one transaction, saves the order plus an outbox event, then responds. The event is published to Kafka keyed by order ID, and restaurant, rider, notification and analytics services react asynchronously and idempotently, while the customer gets live status updates over SSE or WebSockets.

## Key words
`Sync lane` · `Async lane` · `Outbox` · `Kafka` · `Idempotency` · `SSE`
