# 47 · Design a URL shortener (bit.ly)

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [46](../../phase-5-interview-mode/46-system-design-interview-framework/README.md) · ⏭️ [48 · Design a distributed rate limiter](../../phase-6-case-studies/48-design-distributed-rate-limiter/README.md)

## The problem
Oota wants short links for SMS offers: `oo.ta/aZ3kP9q` instead of a 200-character URL. Billions of clicks, links must work for years.

## In one line
**Generate a unique ID, encode it in base62 into ~7 characters, store code → long URL, and serve redirects from a cache because reads outnumber writes 100 to 1.**

☕ **Darshini:** the token number at the counter: a short number stands for your full order slip kept in the kitchen.

## R · Requirements
- Shorten a URL (optional custom alias, expiry) · redirect · basic click counts
- Read-heavy (100:1), very low redirect latency, highly available, durable

## E · Estimation
- 10 crore new URLs/month → **~40 creates/s** · **~4,000 redirects/s** (peak 20k)
- ~600 GB/year · base62, 7 chars → 62⁷ ≈ **3.5 trillion** codes

## A · API
```
POST /v1/urls { long_url, custom_alias?, expires_at? } → { short_url }
GET  /{code} → 301/302 Location: long_url
```

## D · Data model
`urls(code PK, long_url, created_at, expires_at)` — pure key lookups → KV / wide-column / sharded SQL

## H · High-level design
```
Create:   Client → LB → API → ID generator → base62 → DB (+cache)
Redirect: Client → LB → Redirect svc → Redis ─hit→ 302
                                          └miss→ DB → cache → 302
Clicks:   Redirect svc → Kafka → analytics
```

## D · Deep dives
| Code generation | ✅ | ❌ |
|---|---|---|
| Counter/Snowflake + base62 | no collisions | guessable (shuffle the ID) |
| Hash + take 7 chars | same URL → same code | collisions → retry |
| Random + check | unpredictable | needs collision check |
- **301** permanent → browser caches, fast, no analytics · **302** temporary → every click counted, link changeable
- Cache hot links (80/20), negative-cache missing codes, rate-limit creation, scan for phishing

## 🎯 Interview Line
**Q: How would you generate the short codes and why?**
> Use a distributed unique ID generator — pre-allocated ranges or Snowflake — and base62-encode the ID into about 7 characters. It's collision-free and short; to stop people guessing sequential codes I'd permute or encrypt the ID before encoding.

## Key words
`Base62` · `301 vs 302` · `Cache-aside` · `Hot keys` · `Snowflake ID` · `Negative caching`
