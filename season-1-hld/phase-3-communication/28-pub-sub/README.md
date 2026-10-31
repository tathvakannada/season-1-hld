# 28 · Publisher–Subscriber (Pub/Sub)

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [27](../../phase-3-communication/27-message-queues/README.md) · ⏭️ [29 · Kafka in depth](../../phase-3-communication/29-kafka/README.md)

## The problem
When an order is placed, 5 services care: payment, restaurant, rider, notifications, analytics. Next month a 6th (loyalty) is added. Must the order service change every time?

## In one line
**A publisher announces an event to a topic; every subscriber gets its own copy — one event, many independent reactions.**

☕ **Darshini:** the announcement **"Masala dose ready!"** — the waiter, the cashier and the kid at table 4 all react. The cook doesn't call each one.

## Parts
- **Publisher** emits an event: `OrderPlaced {order_id, user_id, total}`
- **Topic** — a named stream (`orders`)
- **Subscribers** — each gets **every** event

Adding loyalty points = a new subscriber. **Order service unchanged.**

## Queue vs Pub/Sub
| | Queue | Pub/Sub |
|---|---|---|
| Who gets a message? | **one** consumer | **every** subscriber |
| Purpose | distribute **work** | broadcast **events** |
| Example | "send this SMS" | "an order was placed" |

Real systems combine them: a topic → several subscriptions → each with competing workers.

## Commands vs events
- **Command:** "do this" (`SendSMS`) → queue
- **Event:** "this happened" (`OrderPlaced`) → topic

> 💡 Commands go to queues; events go to topics.

## Notes
- Durable subscriptions keep messages for offline subscribers; Redis Pub/Sub does **not**.
- Subscribers must be independent (notifications failing ≠ payments failing).

Examples: Kafka, Google Pub/Sub, AWS SNS (+SQS), RabbitMQ exchanges, NATS, Redis Pub/Sub.

## 🎯 Interview Line
**Q: What's the difference between a message queue and pub/sub?**
> A queue delivers each message to one consumer, to distribute work. Pub/sub delivers each event to every subscriber, to broadcast that something happened. Publishing events decouples services — new consumers can be added without changing the publisher.

## Key words
`Publisher` · `Subscriber` · `Topic` · `Subscription` · `Fan-out` · `Command vs event`
