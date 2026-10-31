# 48 · Design a distributed rate limiter

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [47](../../phase-6-case-studies/47-design-url-shortener/README.md) · ⏭️ [49 · Design a notification system](../../phase-6-case-studies/49-design-notification-system/README.md)

## The problem
Oota runs 50 gateway servers. A bot sends 10,000 OTP requests per second, spread across all of them. Each server alone sees only 200/s — so who stops the bot?

## In one line
**Keep shared counters in Redis, update them atomically with a Lua script (token bucket), check them inside every gateway, and decide what happens when Redis is down.**

☕ **Darshini:** one shared register at the entrance of all branches — "3 free coffees per person per day" — so you can't cheat by going to another branch.

## R · Requirements
- Limit per key (user, IP, API key) per rule (e.g., 100/min; OTP 3 per 10 min)
- Adds < 1–2 ms · works across all gateways · highly available · returns **429**

## E · Estimation
1 lakh requests/s → 1 lakh Redis ops/s → small Redis cluster · ~3 crore counters ≈ 3 GB memory

## A · Interface
`allow(key, rule) → allowed, remaining, reset_at` (library inside the gateway) · response: `429` + `Retry-After`

## D · Data
`rl:{rule}:{key}` → `{tokens, last_refill}` with TTL

## H · High-level design
```
Client → API gateways (limiter middleware + cached rules)
             │ atomic Lua check
             ▼
        Redis cluster (sharded by key)      Rules config ──► gateways
allowed → backend      denied → 429
```

## D · Deep dives
- **Race condition:** two gateways read 99 at the same time → both allow → fix with **atomic Lua script**
- **Algorithm:** token bucket (bursty APIs) · sliding window (smooth) · sliding log (exact, low volume: OTP)
- **Redis down?** fail **open** for normal APIs, fail **closed** for OTP/login/payments
- Hot customer key → local allowance synced periodically · multi-region → per-region limits

## 🎯 Interview Line
**Q: How do you rate-limit across many servers?**
> Keep counters in a shared fast store like Redis, sharded by the limited key, and make the check-and-update atomic with a Lua script implementing a token bucket. The check runs as middleware inside each gateway; on Redis failure most endpoints fail open, but sensitive ones like OTP fail closed.

## Key words
`Token bucket` · `Sliding window` · `Lua script` · `Atomicity` · `Fail open` · `Fail closed` · `429`
