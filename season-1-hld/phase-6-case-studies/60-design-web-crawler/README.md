# 60 · Design a web crawler

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [59](../../phase-6-case-studies/59-design-search-autocomplete/README.md) · ⏭️ [61 · Design a payment system (UPI-style)](../../phase-6-case-studies/61-design-payment-system/README.md)

## The problem
A search engine needs a billion pages a month — without hammering any small website, without downloading the same page twice, and without getting stuck in endless calendar links.

## In one line
**A URL frontier hands out URLs by priority but only one at a time per website, fetchers download pages, and Bloom filters and content hashes stop duplicate work.**

☕ **Darshini:** a polite delivery boy covering the whole city: important addresses first, never knocking on the same house 50 times a minute, and ticking off houses already visited.

## R · Requirements
Seeds → fetch → extract links → repeat · re-crawl for freshness
**Polite** (robots.txt, one site at a time) · robust (traps, bad HTML) · scalable

## E · Estimation
1 billion pages/month ≈ **400 pages/s** · 500 KB each → ~500 TB/month

## H · Components
```
Seeds → URL Frontier (priority + per-host queues)
  → Fetchers (DNS cache, robots.txt cache)
  → Content seen? (hash / SimHash) → Store page
  → Extract links → normalise → URL seen? (Bloom filter) → back to Frontier
```

## D · Deep dives
| Part | Idea |
|---|---|
| Front queues | **priority** (important/fresh pages first) |
| Back queues | **politeness**: one queue per host, delay between requests |
| URL dedupe | normalise + **Bloom filter** |
| Content dedupe | hash (exact) + SimHash (near-duplicates) |
| Freshness | re-crawl by change rate, `If-Modified-Since` |
| Traps | max depth/URL length, per-host page caps |
| Scale | partition by **host hash** (consistent hashing) |

## 🎯 Interview Line
**Q: How does a crawler avoid overloading a single website?**
> The URL frontier keeps a separate queue per host, and each host's queue is served by only one fetcher at a time with a delay between requests, respecting robots.txt and crawl-delay. Partitioning the frontier by host hash keeps all of a site's URLs on one crawler node so politeness is enforced locally.

## Key words
`URL frontier` · `Politeness` · `robots.txt` · `Bloom filter` · `SimHash` · `Spider trap` · `Re-crawl`
