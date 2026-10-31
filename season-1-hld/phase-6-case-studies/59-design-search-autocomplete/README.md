# 59 · Design search autocomplete

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [58](../../phase-6-case-studies/58-design-google-drive/README.md) · ⏭️ [60 · Design a web crawler](../../phase-6-case-studies/60-design-web-crawler/README.md)

## The problem
You type "bir" on Oota and "biryani near me" appears before you finish typing. That's crores of keystrokes per hour, each answered in under 100 ms.

## In one line
**Store queries in a trie where every node keeps its precomputed top-k suggestions, build it offline from search logs, serve it from memory, and cache aggressively in the browser and CDN.**

☕ **Darshini:** the waiter who hears "masa…" and already says "masala dosa, masala puri, masala tea" — he memorised the top orders for every starting word.

## R · Requirements
Top 5–10 suggestions per prefix · ranked by popularity (+ freshness, personal) · **< 100 ms** · filter bad suggestions

## E · Estimation
~7,000 RPS avg, peak ~30,000 · trie with top-k fits in memory on a few shards

## A · API
`GET /v1/suggest?q=bir&limit=8&lang=kn`

## D · Data structure: trie
```
        (root)
        /   \
       b     d
       |     |
       i     o
       |      \
       r→ top-k: [biryani near me, birthday cake, …]
```
Store **top-k at every node** → lookup = walk the prefix + read the list

## H · High-level design
```
Read : Browser (debounce, cache) → CDN → Suggest svc → in-memory trie (sharded, replicated)
Write: Search logs → Kafka → aggregate counts → build new trie → swap atomically
```

## D · Deep dives
- Don't update the live trie per search — **rebuild offline**, plus a small **trending** layer for fresh topics
- Shard by prefix ranges (uneven letters!) or hash · replicate for reads
- Client tricks: debounce 150 ms, cancel old requests, cache prefixes
- Blocklist filtering during build + instant takedown at query time

## 🎯 Interview Line
**Q: How do you make autocomplete fast at large scale?**
> Precompute: keep a trie in memory where each node stores its top-k completions, so a request is just walking the prefix and returning a stored list. The trie is rebuilt offline from aggregated search logs and swapped in atomically, and browsers and CDNs cache popular prefixes, with debouncing on the client.

## Key words
`Trie` · `Prefix` · `Top-k` · `Offline build` · `Debounce` · `Trending layer` · `Sharding by prefix`
