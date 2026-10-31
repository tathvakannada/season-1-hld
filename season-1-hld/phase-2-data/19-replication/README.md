# 19 · Replication

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [18](../../phase-2-data/18-nosql-family/README.md) · ⏭️ [20 · Sharding (Partitioning)](../../phase-2-data/20-sharding/README.md)

## The problem
The single DB is overloaded by reads (90% of traffic) and is a single point of failure.

## In one line
**Replication keeps copies of the same data on multiple machines — for availability and read scaling.**

☕ **Darshini:** the manager's **main register** (leader) is the only one written in; **photocopies** (replicas) at each counter are for reading — made every few minutes, so a counter may miss the latest order.

## Leader–follower (primary–replica)
- **All writes → leader.** Leader ships its change log to **followers**.
- **Reads → followers** → add replicas to scale reads.
- Leader dies → **promote** a follower (failover).

## Sync vs async
| | Synchronous | Asynchronous |
|---|---|---|
| Leader waits for replica? | yes | no |
| Data loss on leader crash | none | last few writes |
| Write latency | higher | low |
Common: one sync + others async (**semi-sync**).

## Replication lag problems
- **Read-your-writes broken:** place an order → "My orders" (from a lagging replica) is empty. Fix: read your own recent data from the leader.
- **Monotonic reads broken:** refresh twice, data appears then disappears. Fix: stick a user to one replica.

## Other styles
- **Multi-leader:** several nodes accept writes (multi-region, offline apps) → must resolve **conflicts**.
- **Leaderless (Dynamo-style):** write/read several replicas directly with **quorums** (next videos); repair stale replicas.

## Remember
- Replication ≠ backup (a bad DELETE replicates too).
- Replication = **same data** on many machines. Sharding = **different data** on different machines.

## 🎯 Interview Line
**Q: What is replication lag and what problems does it cause?**
> Async followers trail the leader, so users may not see their own writes or may see data jump back and forth between refreshes. We fix it with read-your-writes routing (read recent own data from the leader), sticky replicas, or reading from replicas that have caught up.

## Key words
`Leader` · `Follower` · `Read replica` · `Sync/async replication` · `Replication lag` · `Read-your-writes` · `Failover` · `Multi-leader` · `Leaderless`
