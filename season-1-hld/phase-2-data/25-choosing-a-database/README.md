# 25 · How to choose a database + Phase 2 recap

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [24](../../phase-2-data/24-storage-types/README.md) · ⏭️ [26 · Sync vs Async communication](../../phase-3-communication/26-sync-vs-async/README.md)

## The problem
"Which database would you use?" — "MongoDB, because it scales" is a weak answer. We need a method.

## In one line
**Choose storage by data shape, access pattern, correctness needs and scale — and use several stores together when it helps.**

☕ **Darshini:** a big hotel uses a **ledger** for money, a **notice board** for today's specials, a **godown** for sacks, and an **index card box** to find recipes — each for its job.

## The 6 questions
1. **Shape** — relational? nested? key-value? graph? files?
2. **Access** — by key? joins? ranges? full-text? read- or write-heavy?
3. **Correctness** — need transactions? or eventual is fine?
4. **Size** — GBs, TBs, PBs? writes per second?
5. **Latency & geography** — sub-ms? multi-region?
6. **Team & operations** — what do we know? managed service?

## Quick map
| Need | Default |
|---|---|
| Orders, payments, users | PostgreSQL / MySQL |
| Sessions, cache, counters, leaderboards | Redis |
| Flexible catalog | MongoDB (or Postgres JSONB) |
| Massive time-ordered writes | Cassandra / DynamoDB |
| Text search | Elasticsearch / OpenSearch |
| Relationships | Neo4j |
| Images, videos, backups | S3 + CDN |
| Analytics | Data warehouse / lake |

> 💡 Start with **Postgres + Redis + S3**; earn every extra database. Keep **one source of truth** per piece of data; sync others via events (Phase 3).

## Phase 2 recap: Oota v3
| Problem | Solution |
|---|---|
| Money deducted, no order | ACID transactions |
| Slow queries | Indexes |
| Data shapes don't fit tables | NoSQL families |
| Read overload, DB SPOF | Replication |
| Too much data / writes | Sharding |
| Adding nodes moves everything | Consistent hashing |
| Partition: correct or available? | CAP / PACELC |
| "Can't see my own order" | Consistency models, quorums |
| DB full of photos | Object storage + CDN |

## 🎯 Interview Line
**Q: Which database would you use for an e-commerce order system?**
> A relational database like PostgreSQL for orders and payments because they need transactions and relations, with read replicas and later sharding by user. Redis for sessions and hot data, a search engine for product search, and object storage plus a CDN for images — each kept in sync from the source of truth via events.

## Key words
`Polyglot persistence` · `Source of truth` · `Access pattern` · `Change data capture`
