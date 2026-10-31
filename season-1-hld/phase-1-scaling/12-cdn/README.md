# 12 · CDN (Content Delivery Network)

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [11](../11-caching-part-2/README.md) · ⏭️ [13 · Proxies & API Gateway](../13-proxies-and-api-gateway/README.md)

## The problem
Oota's servers are in Mumbai. Food photos are heavy. Users far away wait seconds; every image request also eats Oota's bandwidth. Distance = latency, and we can't make light faster.

## In one line
**A CDN is a network of servers in many cities that keeps copies of your content close to users.**

☕ Darshini: the main hotel opens **branches** in Jayanagar, Whitefield and Malleshwaram so people eat nearby.

## How it works
- Your server = **origin**. CDN servers near users = **edge servers**, grouped in **PoPs**.
- User is routed to the **nearest edge**:
  - **Hit** → returned immediately
  - **Miss** → edge fetches from origin, stores it, returns it; the next nearby user gets a hit

```
User (Mysuru) → Edge (Bengaluru) ──hit──► fast
                     │ miss
                     ▼
               Origin (Mumbai)
```

## What goes on a CDN
- ✅ **Static** content: images, videos, JS, CSS, fonts, downloads
- ⚠️ **Dynamic/personal** content (your cart, your orders): usually not cached at the CDN — but the CDN still helps with faster TLS, optimised routes, DDoS protection

## Pull vs push
| Pull CDN | Push CDN |
|---|---|
| Edge fetches from origin on first request | You upload content in advance |
| Most websites | Big files, known releases |

## Controlling it
- `Cache-Control: public, max-age=86400` → cache for a day
- `Cache-Control: private` → browser only, not CDN
- `Cache-Control: no-store` → don't cache
- **Invalidation:** purge API (slow to spread) or better — **versioned file names**: `app.3f9a2c.js`, `dose_v2.jpg` → new URL = instantly fresh

## Benefits
Lower latency · less origin load & bandwidth · better availability · DDoS/security protection
**Real world:** Cloudflare, Akamai, Amazon CloudFront, Fastly. Video streaming relies heavily on CDNs.

## ⚖️ Trade-off corner
Costs money, content can be stale, purges take time, harder debugging. ⚠️ Never cache private data with a `public` header.

## 🎯 Interview Line
**Q: How does a CDN reduce latency?**
> It serves content from edge servers close to the user, so requests travel a much shorter distance; it also terminates TLS near the user and keeps optimised connections to the origin. On a miss the edge fetches from the origin once and serves later users from its cache.

## Key words
`CDN` · `Origin` · `Edge` · `PoP` · `Static vs dynamic` · `Pull vs push` · `Cache-Control` · `Purge` · `Versioned URLs`
