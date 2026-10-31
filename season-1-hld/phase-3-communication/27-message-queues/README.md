# 27 · Message Queues

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [26](../../phase-3-communication/26-sync-vs-async/README.md) · ⏭️ [28 · Publisher–Subscriber (Pub/Sub)](../../phase-3-communication/28-pub-sub/README.md)

## The problem
At 1 PM Oota creates 5,000 SMS jobs per minute; the SMS provider handles 1,000. Direct calls fail. At 4 PM it's idle.

## In one line
**A queue is a buffer between producers and consumers — decoupling services, smoothing spikes, enabling retries.**

☕ **Darshini:** **order slips on a rail**: the cashier clips slips, cooks take one each, a slip is removed only when the dish is done; impossible orders go to the manager's tray.

## Parts
- **Producer** → puts a message in the queue
- **Queue (broker)** → stores it durably
- **Consumer / worker** → takes it, does the work, **acks** it
Each message is processed by **one** consumer (workers share the load).

## Why queues
Decoupling · **load levelling** (spikes wait) · reliability (survive crashes) · scale by adding workers.

## Delivery guarantees
| | How | Risk |
|---|---|---|
| At-most-once | ack before work | may lose messages |
| **At-least-once** | ack after work | **duplicates** (most common) |
| Exactly-once | very hard end-to-end | — |

> 💡 **At-least-once + idempotent consumer = effectively once.**

## Idempotent consumers
Track processed message IDs · idempotent updates (`… WHERE status = 'PENDING'`) · idempotency keys for external calls.

## Retries & Dead Letter Queue
Retry with exponential backoff (1 s, 2 s, 4 s…). After N failures → **DLQ** for inspection, so a "poison message" can't block everything.

## Ordering
Many workers → no strict order. Need order per order_id? → FIFO / partition by key.

Examples: RabbitMQ, Amazon SQS, Redis-based queues (Celery, Sidekiq, BullMQ).

## 🎯 Interview Line
**Q: Why use a message queue, and what is a dead letter queue?**
> A queue decouples producers from consumers, buffers traffic spikes, and enables retries so work isn't lost when a consumer fails. Messages that keep failing after several retries go to a dead letter queue for inspection, so one bad message can't block or loop forever.

## Key words
`Producer` · `Consumer` · `Broker` · `Ack` · `At-least-once` · `Idempotent consumer` · `Retry with backoff` · `Dead letter queue`
