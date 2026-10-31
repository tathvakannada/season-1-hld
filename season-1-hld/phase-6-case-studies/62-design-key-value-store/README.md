# 62 · Design a distributed key-value store

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [61](../../phase-6-case-studies/61-design-payment-system/README.md) · ⏭️ [63 · Design a real-time leaderboard (Dream11)](../../phase-6-case-studies/63-design-leaderboard/README.md)

## The problem
Build your own Redis/DynamoDB: `put(key, value)` and `get(key)` across 100 machines, still working when some machines die, with consistency you can tune.

## In one line
**Place keys on a consistent-hash ring, replicate each key to N nodes, use quorums (W + R > N) for consistency, gossip to detect failures, and Merkle trees to repair replicas.**

☕ **Darshini:** spare keys of every house kept with 3 neighbours on the street: any 2 agreeing is enough, and neighbours chat daily to notice who has moved away.

## R · Requirements
put / get · scale out · highly available (always writable) · tunable consistency · low latency · auto failure handling

## Building blocks (this case study = Phase 2 revision)
| Problem | Technique | Video |
|---|---|---|
| Spread keys | **Consistent hashing** + virtual nodes | 21 |
| Survive failures | **Replication** (N = 3) | 19 |
| Consistency | **Quorum** W + R > N | 23 |
| Concurrent writes | LWW timestamps / version vectors | 23 |
| Temporary failure | **Hinted handoff** | — |
| Permanent failure | **Merkle tree** anti-entropy | — |
| Who's alive? | **Gossip** protocol | — |
| Fast disk writes | **LSM tree** + Bloom filters | 17, 42 |

## H · Architecture
```
Client → coordinator node → hash key → N replicas on the ring
 write: send to N, wait W acks · read: ask R, return newest + read-repair
Each node: commit log → memtable → SSTables → compaction
Background: gossip, hinted handoff, Merkle repair
```

## Quorum examples (N = 3)
| W | R | Result |
|---|---|---|
| 2 | 2 | overlap → strong-ish reads |
| 1 | 1 | fastest, eventual |
| 3 | 1 | durable writes, unavailable if 1 node down |

## 🎯 Interview Line
**Q: How does a Dynamo-style store stay available when a replica is down?**
> With a sloppy quorum and hinted handoff: the write goes to the next healthy node on the ring with a hint saying who it really belongs to, and when the owner comes back the hint is handed over. Gossip spreads failure information, and Merkle-tree anti-entropy repairs any replicas that drifted apart.

## Key words
`Consistent hashing` · `Replication factor` · `Quorum` · `Hinted handoff` · `Gossip` · `Merkle tree` · `LSM tree` · `Vector clock`
