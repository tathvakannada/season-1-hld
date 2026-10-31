# 10 · Caching part 1 — What, where, why

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [09](../09-stateless-vs-stateful/README.md) · ⏭️ [11 · Caching part 2](../11-caching-part-2/README.md)

## The problem
At 12:30 PM, 1 lakh users open "Top restaurants in Bengaluru". The DB runs the **same heavy query 1 lakh times** for the **same answer**.

## In one line
**A cache is a small, fast store that keeps copies of frequently used data so we don't redo expensive work every time.**

☕ Darshini: the **tray of ready idlis** near the counter.

## Why it works
- **Memory is much faster** than a DB query over the network.
- A **small portion of data gets most of the requests** (popular restaurants, today's menu). Cache that hot portion.

## Vocabulary
- **Hit** — found in cache (fast) · **Miss** — go to the DB, then store it
- **Hit ratio** = hits ÷ (hits + misses) — the cache's exam score
- **TTL** — how long an entry lives
- **Eviction** — removing entries when full
- **Stale** — cache has an old copy (part 2!)

## Where caches live
```
Browser/app → CDN → reverse proxy → app memory → Redis/Memcached → DB's own cache → disk
```
| Layer | Example |
|---|---|
| Client | browser cache (`Cache-Control`) |
| CDN | images, JS, CSS |
| App memory (local) | small hot data, config |
| Distributed cache | **Redis**, Memcached — sessions, query results |
| Database | its own buffer cache |

**Local vs distributed:** local is fastest but every server has its own (inconsistent) copy; distributed (Redis) is shared by all servers but costs a network hop.

## What to cache
✅ Read-heavy, rarely changing, expensive to compute (menus, restaurant lists, profiles, sessions)
❌ Fast-changing data that must be exact (wallet balance at payment time), rarely re-read data

## Eviction policies
- **LRU** — remove the *least recently used* (most common)
- **LFU** — remove the *least frequently used*
- **FIFO** — remove the oldest inserted
- **TTL** — expire after a time

💡 LRU in O(1) = **HashMap + doubly linked list** (classic interview question).

## ⚖️ Trade-off corner
Cache = speed, but: memory costs money, data can be **stale**, and the cache can lose data anytime → your code must always work on a miss. **The DB stays the source of truth.**

## 🎯 Interview Line
**Q: What is LRU and why is it common?**
> LRU evicts the item that hasn't been accessed for the longest time. It works well because recently used data is likely to be used again, and it can be implemented in O(1) with a hash map plus a doubly linked list.

## Key words
`Cache` · `Hit/Miss` · `Hit ratio` · `TTL` · `Eviction` · `LRU` · `LFU` · `Redis` · `Local vs distributed cache`
