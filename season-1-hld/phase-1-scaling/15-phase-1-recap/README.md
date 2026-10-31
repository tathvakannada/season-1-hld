# 15 · Phase 1 recap — draw Oota v2 from memory

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [14](../14-spof-redundancy-failover/README.md) · ⏭️ [16 · SQL deep dive: ACID & transactions](../../phase-2-data/16-acid-and-transactions/README.md)

## The whole phase in 8 problems
**Every box exists because a problem forced it in.** Remember the problems → you can redraw the architecture anytime.

| # | Problem Oota hit | Box we added |
|---|---|---|
| 1 | One server died on IPL night | More servers (horizontal scaling) |
| 2 | Users only know one address | Load balancer |
| 3 | Cart/login lost between servers | Stateless servers + Redis session store |
| 4 | Same heavy DB query 1 lakh times | Cache (Redis) |
| 5 | Stale prices, cache stampedes | Cache strategies + invalidation |
| 6 | Far users wait for heavy images | CDN |
| 7 | Every service re-implements auth/limits | API gateway |
| 8 | One DB / LB / data centre dies | Redundancy + failover, multi-AZ |

## Oota v2
```
User ── DNS
  │ └──────────────► CDN (images, JS, CSS)
  ▼
Load balancer pair (multi-AZ)
  ▼
API gateway  (auth · rate limit · routing)
  ▼
App 1 · App 2 · … App N   (stateless, autoscaled, across 2 AZs)
  ├──► Redis (cache + sessions)   primary + replica
  └──► DB primary ──replication──► DB standby (other AZ) + backups
```

## Quick-fire revision
1. Why not keep buying a bigger server? → ceiling, cost, SPOF
2. What does an LB do when a server fails health checks? → removes it
3. Why did the cart disappear? → state in one server's memory
4. Two ways to make servers stateless? → session store, signed tokens
5. Hit ratio 95% means? → 95% of reads served from cache
6. Update the DB, then ___ the cache? → delete
7. What goes on a CDN? → static, cacheable content
8. Forward vs reverse proxy — who is hidden? → client vs server
9. "Two is one, one is ___"? → none
10. Next bottleneck? → **the database** (Phase 2)

## Common mistakes when drawing this
- Only one load balancer / one availability zone
- Apps connecting directly to the database
- Treating the cache as the source of truth
- Sticky sessions instead of stateless servers

## Key words
All of Phase 1: `Horizontal scaling` · `Load balancer` · `Stateless` · `Cache` · `CDN` · `API gateway` · `Redundancy` · `Failover`
