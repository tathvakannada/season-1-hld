# 14 · Single Point of Failure, Redundancy & Failover

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [13](../13-proxies-and-api-gateway/README.md) · ⏭️ [15 · Phase 1 recap](../15-phase-1-recap/README.md)

## The problem
Oota has 10 app servers — but one load balancer, one database, one Redis, in one data centre. The DB disk dies → **everything** is down. Scaling ≠ reliability.

## In one line
**A single point of failure (SPOF) is any part whose failure takes the whole system down. Remove SPOFs with redundancy (spare copies) and failover (automatic switch).**

☕ Darshini: one gas cylinder for the whole hotel. Keep a second one connected with an auto-switch valve.

## Find SPOFs: the "what if this dies?" walk
Go box by box: *"If this dies, does everything stop?"*
Usual suspects: single LB · single DB · single cache (when the DB can't survive without it) · single data centre/region · single payment gateway · single queue broker.

## Redundancy
- **Active–passive** — primary serves, standby waits and takes over. Simpler; standby idle.
- **Active–active** — all copies serve. Better use and faster failover; harder for data (write conflicts).
- **N+1** — if you need N servers at peak, run N+1.

## Failover
1. **Detect** — heartbeats / health checks.
2. **Switch** — move the IP/DNS, promote a replica, LB stops routing to the dead node.
⚠️ **Split brain** — a slow node is wrongly declared dead → two primaries → conflicting writes. (Phase 4.)
⚠️ With async replication, the last few seconds of writes may be lost.
⚠️ **Untested failover doesn't work** — practise it (chaos engineering).

## Spread across locations
- **Availability Zones (AZs)** — separate data centres in one region. Run in ≥ 2.
- **Multi-region** — survives a whole region outage; much more complex.

## Disaster recovery words
- **Backups** (and tested restores!) — a replica is **not** a backup (it copies mistakes too).
- **RPO** — how much data can we lose? · **RTO** — how long can we be down?

## Graceful degradation
If recommendations are down → show "popular restaurants". Don't let a small failure become a total failure.

> 💡 **"Two is one, one is none."**

## 🎯 Interview Line
**Q: How would you remove single points of failure from this design?**
> Walk every component and ask what happens if it fails. Add redundancy: redundant or managed load balancers, multiple app servers across availability zones, a database replica with automatic failover, a replicated cache, and backups. Use health checks for detection and test failover regularly.

## Key words
`SPOF` · `Redundancy` · `Failover` · `Active-passive` · `Active-active` · `Availability zone` · `Split brain` · `RPO/RTO` · `Graceful degradation`
