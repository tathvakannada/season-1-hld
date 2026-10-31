# 45 · Back-of-the-envelope estimation

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [44](../../phase-4-architecture/44-phase-4-recap/README.md) · ⏭️ [46 · The System Design Interview framework](../../phase-5-interview-mode/46-system-design-interview-framework/README.md)

## The problem
Interviewer says "Design Instagram". One database or a hundred? Cache or not? 1 GB or 1 PB? Without rough numbers every choice is a guess.

## In one line
**Turn vague scale into rough numbers — requests per second, storage, bandwidth, servers — in two minutes, using round numbers. The right order of magnitude matters, not precision.**

☕ **Darshini:** deciding how many idlis to steam: customers per morning × idlis each × rush-hour factor. Nobody counts to the last idli.

## Handy numbers
| Fact | Use |
|---|---|
| 1 day = 86,400 s ≈ **10⁵ s** | per day ÷ 10⁵ = per second |
| 1 lakh = 10⁵ · 1 crore = 10⁷ · 100 crore = 10⁹ | Indian ↔ powers of 10 |
| KB 10³ · MB 10⁶ · GB 10⁹ · TB 10¹² · PB 10¹⁵ | sizes |
| ID/timestamp 8 B · short text ~200 B · DB row ~1 KB · photo ~1 MB | item sizes |
| app server: 100s–1000s RPS · DB: 1000s–10,000s QPS · Redis ~100k ops/s | capacity |

## The 4-step method
1. **Daily actions** = DAU × actions per user
2. **QPS** = daily ÷ 10⁵ → **peak** = avg × 2–10
3. **Storage** = items/day × size × days × 3 (replicas)
4. **Bandwidth** = QPS × size · **Servers** = peak QPS ÷ per-server capacity
Also write the **read:write ratio** — it decides caching.

## Worked example — Oota
| Quantity | Maths | Result |
|---|---|---|
| Orders | 1 crore DAU × 10% | 10⁶/day |
| Order QPS | 10⁶ ÷ 10⁵ | **10/s** (peak ~100/s) |
| Menu reads | 10⁷ × 20 ÷ 10⁵ | **2,000/s** (peak ~20k/s) |
| Storage | 10⁶ × 1 KB | 1 GB/day → ~1 TB/yr with replicas |
| Rider GPS | 1 lakh riders ÷ 4 s | **25,000 writes/s** ← the real heavy path |

> 💡 Every number must lead to a decision: "100 writes/s → one primary DB is fine", "25k GPS writes/s → separate store".

## Mistakes to avoid
- Precision theatre (4 decimal places) · forgetting peak · forgetting replicas · numbers that never change the design

## 🎯 Interview Line
**Q: Estimate the QPS for a service with 1 crore daily users making 50 requests each per day.**
> 5 × 10⁸ requests per day divided by about 10⁵ seconds gives roughly 5,000 requests per second on average, and with a 5–10× peak factor I'd design for 25,000–50,000 per second — so multiple stateless app servers behind a load balancer plus caching.

## Key words
`DAU` · `QPS` · `Peak factor` · `Read:write ratio` · `Order of magnitude` · `Replication factor`
