# 36 · Rate Limiting

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [35](../../phase-4-architecture/35-microservice-resilience-patterns/README.md) · ⏭️ [37 · Distributed transactions: 2PC and Saga](../../phase-4-architecture/37-distributed-transactions-saga/README.md)

## The problem
A bot tries 10,000 coupon codes a second, scrapers download every menu, a buggy app retries in a loop — servers melt and real users get errors.

## In one line
**A rate limiter caps how many requests a client can make per time window — protecting against abuse, overload and runaway costs.**

☕ **Darshini:** **"one free kesari bath per person"** — and with a token bucket, each customer gets 3 tokens, one new token every 10 minutes.

## Why
Abuse (brute force, OTP spam, scraping) · fairness · cost control (SMS, AI calls) · protect downstream services.

## Where & by what key
Usually at the **API gateway**. Key by user, API key, IP, phone, endpoint — often combined (OTP: per phone **and** per IP).

## Algorithms
| Algorithm | Idea | Bursts | Notes |
|---|---|---|---|
| **Token bucket** | tokens refill at rate R, bucket size B | allowed up to B | most common |
| **Leaky bucket** | queue drained at constant rate | smoothed | steady output |
| **Fixed window** | count per minute | ⚠️ 2× at window edges | simplest |
| **Sliding window log** | store timestamps | none | exact, memory-heavy |
| **Sliding window counter** | weighted current + previous window | small | good compromise |

## Distributed rate limiting
Shared counters in **Redis** with **atomic** operations (INCR + EXPIRE, Lua scripts). Redis down → **fail open** (normal APIs) or **fail closed** (login/OTP).

## Response
`429 Too Many Requests` + `Retry-After` (and `X-RateLimit-*` headers).

## 🎯 Interview Line
**Q: Token bucket vs leaky bucket — and how do you rate-limit across many servers?**
> A token bucket refills tokens at a fixed rate up to a capacity, so it allows short bursts while enforcing an average rate; a leaky bucket processes requests at a constant rate, smoothing bursts. Across many servers, keep counters in a shared store like Redis and update them atomically, for example with a Lua script.

## Key words
`Rate limit` · `Token bucket` · `Leaky bucket` · `Fixed window` · `Sliding window` · `429` · `Fail open / fail closed`
