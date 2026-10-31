# 06 · How do we measure a system? (latency, throughput, availability, scalability)

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [05](../05-databases-101/README.md) · ⏭️ [07 · Vertical vs Horizontal scaling](../../phase-1-scaling/07-vertical-vs-horizontal-scaling/README.md)

## In one line
**Latency** = how long one request takes · **Throughput** = how many requests per second · **Availability** = % of time it works · **Scalability** = how well it handles growth.

☕ Darshini: order → plate time (latency) · plates per hour (throughput) · open when it should be (availability) · add staff for festival rush (scalability).

## Latency
Time for **one** request: network → waiting in queue → code → database → network back.
To speed up, find the **biggest piece first**. Measure, don't guess.

### Averages lie → use percentiles
99 requests take 100 ms, 1 takes 10 s → average ≈ 199 ms (looks fine!) but p99 = 10 s.
- **p50** — half the requests are faster than this
- **p99** — 99% are faster than this
- Targets look like: *"p99 < 300 ms"*

## Throughput
How **many** requests per second (RPS/QPS). Not the same as latency:
a wide highway carries thousands of cars per hour (throughput), but each car still takes an hour to reach Mysuru (latency).

### Bonus: Little's Law
> **Requests in flight = throughput × latency**
1,000 req/s × 0.2 s = **200 requests** in the system at any moment. If one server handles ~50 at a time → you need ~4 servers (+ spare).

## Availability — the nines
| Availability | Downtime per year |
|---|---|
| 99% | ≈ 3.65 days |
| 99.9% | ≈ 8.8 hours |
| 99.99% | ≈ 53 minutes |
| 99.999% | ≈ 5 minutes |

Each extra nine is roughly 10× harder and costlier.

### Series vs parallel
- **Series** (request passes through all): multiply → worse. LB 99.99% × App 99.9% × DB 99.9% ≈ **99.79%**
- **Parallel** (any one copy is enough): `1 − (1 − A)ⁿ` → better. Two 99% servers → **99.99%**
➡️ Redundancy improves availability — the big idea of Phase 1.

## Available vs reliable vs durable
- **Available** — is it up?
- **Reliable** — does it give the *right* answer?
- **Durable** — does saved data stay saved?

## Scalability
Handle more load by adding resources.
- Slow for **one** user → *performance* problem.
- Fast for one, slow for **many** → *scalability* problem.

## Numbers every engineer should know (rough)
| Operation | Roughly |
|---|---|
| RAM read | ~100 ns |
| SSD random read | ~100 µs |
| Round trip, same data centre | ~0.5 ms |
| Round trip India ↔ USA | ~200 ms |

Remember the **gaps**, not exact values — they explain why **caching** and **CDNs** exist.

## ⚖️ Trade-off corner
- Design for the **peak**, not the average (Oota: avg 200 req/s, lunch peak 2,000 req/s).
- Every extra nine costs money — choose what the business actually needs.

## 🎯 Interview Line
**Q: What does 99.99% availability mean? Latency vs throughput?**
> 99.99% allows about 52–53 minutes of downtime a year. Latency is the time one request takes; throughput is how many requests we handle per second. Adding servers raises throughput; lowering latency needs faster processing, caching, or being closer to users. We track p95/p99, not just the average.

## Key words
`Latency` · `Throughput` · `p50/p99` · `Little's Law` · `Availability` · `Nines` · `Reliability` · `Durability` · `Scalability`
