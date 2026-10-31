# 42 · Special data structures: Bloom filter, HyperLogLog, Geohash, Trie

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [41](../../phase-4-architecture/41-security-basics/README.md) · ⏭️ [43 · Deployments & infra: containers, Kubernetes, CI/CD, canary](../../phase-4-architecture/43-deployments-and-infrastructure/README.md)

## The problem
"Is this username taken?" on every keystroke · "How many unique users today?" · "Restaurants within 3 km" · "Suggest dishes as I type 'bir…'" — all at huge scale.

## In one line
**A few clever structures trade a little accuracy or memory for massive savings at scale.**

☕ **Darshini:** a tiny slate of regulars (Bloom filter) · guessing the breakfast crowd size (HyperLogLog) · the delivery map split into zones (geohash) · the menu arranged letter by letter (trie).

## Bloom filter — "definitely not" or "maybe"
Bit array + k hash functions. Any bit 0 → **definitely not present**; all 1 → **maybe**. No false negatives, some false positives; ~10 bits/item ≈ 1% false positives.
Uses: username pre-check, cache penetration, LSM-tree DBs skipping disk reads, crawlers.

## HyperLogLog — "about how many unique?"
Estimates distinct count in tiny fixed memory (Redis: ~12 KB per key, ~0.81% error). `PFADD`, `PFCOUNT`.
Uses: daily unique users, unique searchers.

## Geohash & Quadtree — "what's nearby?"
- **Geohash:** lat/lng → short string; nearby places share a prefix (5 chars ≈ few km, 7 ≈ ~150 m). Search your cell **+ 8 neighbours**, then filter by exact distance.
- **Quadtree:** split the map into 4 until each cell has ≤ K places — adapts to density.
- Also: R-trees (PostGIS), Google S2, Uber H3.

## Trie — "what starts with…?"
Prefix tree; store **top-K suggestions** at each node (precomputed) → instant autocomplete.

| Structure | Answers | Oota use |
|---|---|---|
| Bloom filter | in the set? (maybe) | username taken? |
| HyperLogLog | how many unique? | daily active users |
| Geohash / Quadtree | what's nearby? | restaurants within 3 km |
| Trie | what starts with…? | search autocomplete |

## 🎯 Interview Line
**Q: How would you find restaurants within 3 km of a user?**
> Index restaurants by geohash (or a quadtree). For a user, compute their geohash cell, fetch restaurants in that cell and its 8 neighbours with a prefix query, then filter by exact distance and sort by distance or rating. Redis GEO commands or PostGIS can do this out of the box.

## Key words
`Bloom filter` · `False positive` · `HyperLogLog` · `Cardinality` · `Geohash` · `Quadtree` · `Trie` · `Top-K`
