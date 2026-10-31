# 38 · Unique ID generation at scale

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [37](../../phase-4-architecture/37-distributed-transactions-saga/README.md) · ⏭️ [39 · Consensus, leader election & distributed locks](../../phase-4-architecture/39-consensus-leader-election-locks/README.md)

## The problem
Orders are sharded across 16 databases; each shard's auto-increment gives out ID 1023 → duplicates. A central ID database becomes a bottleneck and single point of failure.

## In one line
**Generate IDs independently on every machine without collisions — random UUIDs or time-sortable IDs like Snowflake.**

☕ **Darshini:** every branch prints tokens as **date-time + branch code + counter** ("1231-JYN-042") — no branch needs to call head office for a unique number.

## Options
| | Size | Sortable | Coordination | Notes |
|---|---|---|---|---|
| Auto-increment | 64-bit | ✅ | central DB | SPOF, one shard |
| UUID v4 | 128-bit | ❌ | none | random, index-unfriendly |
| **UUID v7 / ULID** | 128-bit | ✅ | none | modern default |
| Ticket ranges | 64-bit | ~ | central service | blocks of IDs |
| **Snowflake** | 64-bit | ✅ by time | machine ID | clock-sensitive |

## Snowflake layout (64 bits)
```
| 1 sign | 41 timestamp (ms) | 10 machine ID | 12 sequence |
```
- 41 bits ≈ **69 years** · 10 bits = **1,024** machines · 12 bits = **4,096 IDs per ms per machine**
- ⚠️ clock moves backwards → wait/refuse to avoid duplicates

## Public vs internal IDs
Sequential IDs in URLs leak volume and invite guessing → use unguessable public IDs and **always check authorisation**.

## 🎯 Interview Line
**Q: How would you generate unique IDs in a distributed system?**
> I'd clarify size, ordering and throughput needs. For 64-bit, time-sortable IDs generated locally I'd use a Snowflake layout — 41-bit timestamp, 10-bit machine ID, 12-bit sequence — handling clock drift by refusing to go backwards. If 128 bits is fine, UUID v7 gives sortable IDs with no coordination.

## Key words
`Auto-increment` · `UUID v4` · `UUID v7` · `Snowflake` · `Machine ID` · `Sequence` · `Clock skew`
