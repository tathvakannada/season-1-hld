# 20 · Sharding (Partitioning)

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [19](../../phase-2-data/19-replication/README.md) · ⏭️ [21 · Consistent Hashing](../../phase-2-data/21-consistent-hashing/README.md)

## The problem
The `orders` table is 5 TB and growing; all writes still hit one leader. One machine can't store or write it all.

## In one line
**Sharding splits data into pieces stored on different machines; the shard key decides where each row goes.**

☕ **Darshini:** one store room can't hold everything → **separate store rooms** (rice items, snacks, drinks). The labelling rule = the shard key.

## Vertical vs horizontal partitioning
- **Vertical:** split tables/columns (orders DB, users DB)
- **Horizontal = sharding:** split **rows** of one table across machines

## A good shard key is "EQS"
- **E**ven — spreads data & traffic
- **Q**uery-friendly — most queries hit **one** shard
- **S**table — doesn't change

## Strategies
| Strategy | ✅ | ❌ |
|---|---|---|
| **Range** (IDs 1–1M → shard 1…) | range queries easy | hot spots (newest data on one shard) |
| **Hash** (`hash(key) % N`) | even spread | ranges scatter; changing N moves most data |
| **Directory** (lookup table) | flexible | extra hop; lookup must be HA |
| **Geo** (by region) | data near users | uneven regions |

## Problems sharding creates
- **Hot shards** (celebrity restaurant) → split the key, cache, dedicate a shard
- **Cross-shard queries** → scatter-gather, slow → precompute
- **Cross-shard joins/transactions** → avoid, denormalize, Sagas
- **Resharding** → consistent hashing (next video) or many logical shards
- **Global unique IDs** → Snowflake IDs (Phase 4)

> 💡 Shard **last**: scale up → replicas → cache → archive old data → then shard.

## 🎯 Interview Line
**Q: How would you choose a shard key for an orders table?**
> Pick the key that matches the main query — usually user_id, so a user's orders live on one shard — and that spreads data evenly and never changes. Avoid ever-increasing keys like timestamps for range sharding, and build separate read models for other access patterns like 'orders by restaurant'.

## Key words
`Shard` · `Shard key` · `Range sharding` · `Hash sharding` · `Directory sharding` · `Hot shard` · `Scatter-gather` · `Resharding`
