# 65 · Season 1 finale — the whole map

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [64](../../phase-6-case-studies/64-design-job-scheduler/README.md)

## The problem
65 videos, ~150 concepts, 18 systems. How does it all connect — and how do you keep it in your head for an interview?

## In one line
**Every big system is a remix of the same 15 patterns. Learn the patterns, and any new system becomes a combination you already know.**

☕ **Darshini:** a darshini cook doesn't memorise 200 dishes — he knows a few batters, chutneys and techniques and combines them.

## The Season 1 map
| Phase | Videos | You can now… |
|---|---|---|
| 0 Foundations | 1–6 | trace a request; APIs; databases; key numbers |
| 1 Scaling | 7–15 | scale out, load balance, cache, CDN, remove SPOFs |
| 2 Data | 16–25 | transactions, indexes, NoSQL, replication, sharding, CAP |
| 3 Communication | 26–33 | queues, pub/sub, Kafka, events, real-time, APIs |
| 4 Architecture | 34–44 | microservices, resilience, rate limits, sagas, IDs, consensus |
| 5 Interview mode | 45–46 | estimation + R-E-A-D-H-D |
| 6 Real systems | 47–64 | 18 systems end to end |

## 15 patterns that solve 90% of problems
1. Read-heavy → cache + CDN + read replicas
2. Write-heavy → log/queue, batch, LSM, shard
3. Huge data → shard with a good key, consistent hashing
4. Must not lose → replicate + durable log + backups
5. Must not repeat → idempotency keys + unique constraints
6. Across services → saga + outbox + reconciliation
7. Slow work → async queue + workers
8. Fan-out → push for most, pull for celebrities
9. Real-time → WebSockets/SSE + connection registry
10. Hot item → lock / conditional update / hold with TTL
11. Spikes → pre-scale, rate-limit, queue, degrade
12. Nearby → geohash / quadtree / H3
13. Top-k / rank → sorted sets, heaps, precomputed top-k
14. Big files → chunk + hash + object storage + CDN
15. Things fail → timeouts, retries + jitter, circuit breakers, observability

## Case studies in one line
URL shortener: ID → base62 · Rate limiter: token bucket in Lua · Notifications: priority lanes · Chat: WS gateways + registry · Feed: hybrid fan-out · YouTube: parallel transcode + ABR · Live: collapse requests · Uber: geo cells + CAS · Oota: saga + state machine · Booking: hold → pay → confirm · Drive: block hashes + journal · Autocomplete: trie top-k · Crawler: polite frontier · Payments: idempotency + ledger · KV store: ring + quorum · Leaderboard: sorted set · Scheduler: leases + idempotent jobs

## What's next — Season 2
Low-level design: OOP, SOLID, design patterns, UML, and LLD problems (parking lot, elevator, Splitwise, LRU cache…)

## 🎯 Interview Line
**Q: Pick any system and tell me which core patterns it uses.**
> Take food delivery: browsing is read-heavy so it's cached with a CDN; checkout uses idempotency keys; order and payment are coordinated with a saga and outbox; status updates flow as events through Kafka; tracking uses WebSockets; partners are found with a geo index; and the 8 pm spike is handled with pre-scaling, rate limiting and graceful degradation.

## Key words
`Pattern` · `Trade-off` · `R-E-A-D-H-D` · `Season 2: LLD`
