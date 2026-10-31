# 29 · Kafka in depth

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [28](../../phase-3-communication/28-pub-sub/README.md) · ⏭️ [30 · Event-driven architecture (event sourcing, CQRS, outbox)](../../phase-3-communication/30-event-driven-architecture/README.md)

## The problem
Millions of events per second (orders, GPS, clicks), many teams reading the same events, order per order_id, and a new team wants to **re-read last week's events**. A normal queue deletes messages after reading.

## In one line
**Kafka is a durable, distributed, append-only log: topics split into partitions, consumers track offsets, events are kept and can be replayed.**

☕ **Darshini:** Kafka is the counter's **CCTV recording**, not the order slips — kept for a week, and anyone can watch from any point at their own speed.

## Vocabulary
- **Broker** — a Kafka server; a **cluster** has many
- **Topic** — named stream (`orders`)
- **Partition** — an ordered, append-only log; unit of parallelism & ordering
- **Offset** — position inside a partition (0, 1, 2…)
- **Producer** — picks partition by **key** (`order_id`) → same key, same partition, in order
- **Consumer group** — each partition is read by **one** consumer in the group; different groups each get **all** events

## Key rules
- Ordering **only within a partition** → use a key
- **Retention:** events kept for days (or compacted per key) → **replay** by rewinding offsets
- **Replication:** each partition has a leader + followers; `acks=all` for durability
- Max useful consumers in a group = number of partitions

## Kafka vs a traditional queue
| | Queue (RabbitMQ/SQS) | Kafka |
|---|---|---|
| After reading | deleted | kept (retention) |
| Replay | no | yes |
| Many readers | needs fan-out setup | natural (groups) |
| Best for | task queues, jobs | event streams, analytics, CDC |

## Uses
Event backbone between services · clickstream → data lake · **CDC** (DB changes → Kafka) · stream processing (Flink, Kafka Streams).

⚠️ Not for small apps (heavy to operate), per-message delays/priorities, or request/response.

## 🎯 Interview Line
**Q: How does Kafka guarantee ordering, and how do you scale consumers?**
> Ordering is guaranteed only within a partition, so we use a key like order_id to send related events to the same partition. Consumers scale through consumer groups — each partition goes to one consumer in a group — so the number of partitions caps the parallelism.

## Key words
`Broker` · `Topic` · `Partition` · `Offset` · `Consumer group` · `Retention` · `Replay` · `Log compaction` · `CDC`
