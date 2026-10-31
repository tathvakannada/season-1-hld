# 07 · Vertical vs Horizontal Scaling

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [06](../../phase-0-foundations/06-measuring-a-system/README.md) · ⏭️ [08 · Load Balancing](../08-load-balancing/README.md)

## The problem
IPL final, innings break. Everyone orders biryani. Oota's single server hits 100% CPU, requests pile up, the server runs out of memory and **dies**. We need more capacity.

## In one line
**Vertical scaling = make one machine bigger. Horizontal scaling = add more machines.**

☕ Darshini: give the one cook a bigger stove and four hands (vertical) vs hire more cooks (horizontal).

## Vertical scaling (scale up)
More CPU, RAM, faster disk on the **same** machine.
- ✅ Simple — no code change, no distributed-system problems
- ❌ Hard **ceiling** — the biggest machine is finite
- ❌ Gets **very expensive** at the top
- ❌ Still a **single point of failure**
- ❌ Often needs downtime to upgrade

## Horizontal scaling (scale out)
Add more machines and split the work.
- ✅ Practically **no ceiling**
- ✅ **Fault tolerant** — one dies, others continue
- ✅ Cheap machines, **autoscaling** on the cloud
- ❌ Needs a **load balancer** (next video)
- ❌ Servers must be **stateless** (video 09)
- ❌ The **database** becomes the next bottleneck (Phase 2)

| | Vertical | Horizontal |
|---|---|---|
| How | Bigger machine | More machines |
| Limit | Hardware ceiling | Practically none |
| Failure | Single point of failure | Survives failures |
| Complexity | Low | High |
| Scaling downtime | Often | Usually none |

## What real companies do
**Both.** Stateless app servers scale **horizontally**; the database usually scales **vertically first**, then with replicas and sharding.
Planned spikes (sales, IPL) are often **pre-scaled**, because autoscaling takes minutes to react.

> 💡 "Scale up until it hurts, then scale out."
> 💡 Horizontal scaling doesn't remove problems — it **moves** them to the data layer.

## 🎯 Interview Line
**Q: Vertical vs horizontal — what would you do first for a startup?**
> Scale vertically first because it's simple and engineering time is expensive — especially for the database. Move the stateless app tier to horizontal scaling behind a load balancer when we need fault tolerance or hit machine limits.

## Key words
`Vertical scaling` · `Horizontal scaling` · `Autoscaling` · `Elasticity` · `Single point of failure`
