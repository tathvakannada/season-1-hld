# 63 · Design a real-time leaderboard (Dream11)

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [62](../../phase-6-case-studies/62-design-key-value-store/README.md) · ⏭️ [64 · Design a distributed job scheduler](../../phase-6-case-studies/64-design-job-scheduler/README.md)

## The problem
5 crore fantasy teams in one contest. Kohli hits a six and lakhs of teams' scores change at once — and everyone keeps refreshing to see "my rank".

## In one line
**Keep scores in Redis sorted sets for instant top-N and my-rank queries, batch the burst updates, cache the top 100, and compute the final prize ranks exactly from the durable database.**

☕ **Darshini:** the cricket scoreboard at the darshini TV: the top 10 are written big for everyone; your own rank you ask the cashier, who checks the sorted register.

## R · Requirements
Update scores live · top N · my rank (+ neighbours) · per contest · **final ranks exact** (prizes!)

## E · Estimation
5 crore entries ≈ 5 GB in memory · 10⁵+ reads/s during exciting overs · bursty updates

## Why not SQL?
`COUNT(*) WHERE score > mine` on 5 crore rows per request → too slow

## Redis sorted set
| Need | Command | Cost |
|---|---|---|
| Update score | `ZADD` / `ZINCRBY` | O(log n) |
| Top 100 | `ZREVRANGE lb 0 99` | O(log n + 100) |
| My rank | `ZREVRANK lb me` | O(log n) |

## H · High-level design
```
Ball-by-ball events → Scoring svc → Kafka → update workers (batched ZADD)
Leaderboard API → Redis (replicas) · top-100 cached 1–2 s
Durable DB → final exact ranking after the match → prizes (payments)
```

## D · Deep dives
- **Burst:** one six changes lakhs of teams → batch updates, refresh ranks every 30–60 s
- **Too big for one node:** shard by score range, or scatter-gather top N; approximate % rank for the long tail
- **Ties:** composite score (points + earlier time wins)
- **Live = fast & fresh-ish · final = exact & audited**

## 🎯 Interview Line
**Q: How do you return a user's rank among crores of players quickly?**
> Store scores in a Redis sorted set, which is backed by a skip list, so ZREVRANK returns a rank in O(log n) and ZREVRANGE returns the top N. Cache the top 100 for a second or two since it's the same for everyone, and compute the final prize ranking exactly from the durable database after the match.

## Key words
`Sorted set` · `Skip list` · `ZADD / ZREVRANK` · `Top-N cache` · `Score-range sharding` · `Tie-break` · `Approximate rank`
