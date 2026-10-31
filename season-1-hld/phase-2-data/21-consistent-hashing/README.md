# 21 · Consistent Hashing

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [20](../../phase-2-data/20-sharding/README.md) · ⏭️ [22 · CAP theorem (and PACELC)](../../phase-2-data/22-cap-theorem/README.md)

## The problem
Cache nodes 4 → 5 with `hash(key) % N` → about **80% of keys** move → cache misses flood the DB.

## In one line
**Put servers and keys on a ring; each key goes to the next server clockwise — adding/removing a server moves only ~1/N of keys.**

☕ **Darshini:** musical chairs: with modulo, adding one chair makes **everyone** change seats; with consistent hashing only the person next to the new chair moves.

## Why modulo breaks
Going from N to N+1 nodes moves about N/(N+1) of keys (4 → 5 ≈ 80%).

## The hash ring
1. Bend the hash range into a **circle**
2. Hash each **server** → a point on the ring
3. Hash each **key** → a point on the ring
4. Key belongs to the **first server clockwise**

- **Add a server:** only keys in its arc move to it
- **Remove a server:** only its keys move to the next server
- On average **~1/N** of keys move

## Virtual nodes (vnodes)
Each server sits at **many** points on the ring:
- even load distribution
- a dead server's keys spread over **many** servers, not one neighbour
- bigger machines get more vnodes

## Replication on the ring
Store each key on the next **R** servers clockwise (e.g. R = 3).

## Where it's used
Distributed caches, Dynamo-style DBs (Cassandra, DynamoDB's ancestor Dynamo), sticky load balancing, distributed storage.

⚠️ It spreads **keys**, not traffic per key — hot keys still need special handling.

## 🎯 Interview Line
**Q: Explain consistent hashing and why it's needed.**
> With hash % N, changing N remaps most keys. Consistent hashing puts servers and keys on a ring and assigns each key to the next server clockwise, so adding or removing a server only moves the keys in its arc — about 1/N. Virtual nodes give each server many positions for even load and smooth rebalancing.

## Key words
`Hash ring` · `Modulo hashing` · `Virtual nodes` · `Rebalancing` · `Replication factor`
