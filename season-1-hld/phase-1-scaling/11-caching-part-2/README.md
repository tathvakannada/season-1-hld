# 11 · Caching part 2 — Strategies & invalidation

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [10](../10-caching-part-1/README.md) · ⏭️ [12 · CDN](../12-cdn/README.md)

## The problem
A restaurant changes dosa price from ₹60 to ₹80. The DB is updated, but the cache still shows ₹60 for 10 minutes. Users see ₹60, get charged ₹80. 😡

> *"There are only two hard things in Computer Science: cache invalidation and naming things."* — Phil Karlton

## Strategies
| Strategy | How | Good for | Risk |
|---|---|---|---|
| **Cache-aside** (lazy loading) | App checks cache → on miss reads DB → stores in cache | Most web apps | Stale until TTL/invalidation |
| **Read-through** | Cache itself loads from DB on a miss | Cleaner code | Less control |
| **Write-through** | Write cache **and** DB together | Read-after-write data | Slower writes |
| **Write-back** | Write cache now, DB **later** | Counters, likes, views | **Data loss** if cache dies |
| **Write-around** | Write DB only; cache filled on next read | Write-once data | Miss after write |

```python
def get_menu(id):
    menu = cache.get(f"menu:{id}")
    if menu is None:                 # miss
        menu = db.get_menu(id)
        cache.set(f"menu:{id}", menu, ttl=600)
    return menu
```

## Invalidation (keeping the cache honest)
1. **TTL** — accept staleness up to the TTL.
2. **Delete on write** — update DB **then delete** the cache key. (Delete, not update — avoids race conditions.)
3. **Event-driven** — DB change events trigger cache deletes (Phase 3).
4. **Versioned keys** — `menu:42:v7`; bump the version on change.

> 💡 Mantra: **"Update DB, then delete cache."**
> 💡 **"Show from cache, decide from DB."** Oota re-checks the price from the DB at checkout.

## Classic cache problems
- **Stampede / thundering herd** — hot key expires, thousands of requests hit the DB at once → fix with locks/single-flight, early refresh, random TTL jitter.
- **Penetration** — requests for keys that don't exist always miss → cache "not found" briefly, or use a Bloom filter.
- **Avalanche** — many keys expire together / cache cluster down → random TTLs, HA cache.
- **Hot key** — one key gets huge traffic → replicate it or add a local cache layer.

## ☕ Darshini
Cache-aside = waiter checks the tray, if empty fetches from the kitchen and refills the tray. Write-back = put idlis on the tray now, update the register later (if the tray falls, the register is wrong!).

## 🎯 Interview Line
**Q: How do you keep cache and DB consistent?**
> Usually cache-aside with "update the DB, then delete the cache key", plus a TTL as a safety net. For stronger needs, write-through or event-driven invalidation. We accept slight staleness for display, but critical decisions like payments read from the DB.

## Key words
`Cache-aside` · `Read-through` · `Write-through` · `Write-back` · `Write-around` · `Invalidation` · `Stampede` · `Penetration` · `Hot key`
