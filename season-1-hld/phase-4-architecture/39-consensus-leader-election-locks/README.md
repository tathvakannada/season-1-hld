# 39 · Consensus, leader election & distributed locks

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [38](../../phase-4-architecture/38-unique-id-generation/README.md) · ⏭️ [40 · Observability: logs, metrics, traces, alerts](../../phase-4-architecture/40-observability/README.md)

## The problem
The daily payouts job ran on 3 servers → restaurants paid 3 times. After a network glitch, two DB nodes both acted as primary (split brain).

## In one line
**Consensus lets machines agree on one value — like who's the leader — using majority votes; leader election and safe locks build on it.**

☕ **Darshini:** staff **vote for a shift manager**; whoever gets the majority runs the shift; if the manager goes silent, a new vote happens. A minority stuck in the godown can't elect their own manager.

## Majority is the trick
Decisions need **⌊N/2⌋ + 1** votes; two majorities always overlap. **3 nodes tolerate 1 failure, 5 tolerate 2** — use odd numbers.

## Raft in brief
- Roles: follower · candidate · leader; time in **terms**
- No heartbeat → become candidate → ask for votes → **majority** = leader
- Leader replicates the log; an entry is committed when a **majority** has it
- Old leader sees a higher term → steps down
Used in **etcd** (Kubernetes), **Consul**, Kafka **KRaft**, CockroachDB. ZooKeeper uses a similar protocol (ZAB).

## Leader election for your services
Compete for a **lease** (key with TTL) in etcd/ZooKeeper/Consul; the holder runs the job and renews; if it dies, the lease expires.

## Distributed locks
- Redis `SET key value NX PX 30000` — fine for **efficiency**
- For **correctness**: a paused process may outlive its lock → use **fencing tokens** (increasing numbers checked by the resource) + consensus-based stores
- Often better: avoid locks — unique constraints, idempotency, conditional updates

## Split brain prevention
Majority quorums · leases · fencing.

## 🎯 Interview Line
**Q: How do you make sure a scheduled job runs on only one of many instances?**
> Use leader election through a coordination service like etcd or ZooKeeper: instances compete for a lease with a TTL and only the holder runs the job, renewing while alive. As a safety net the job itself is idempotent — for example a unique constraint on restaurant and date for payouts.

## Key words
`Consensus` · `Quorum` · `Raft` · `Term` · `Leader election` · `Lease` · `Distributed lock` · `Fencing token` · `Split brain`
