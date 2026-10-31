# 01 · What is System Design?

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏭️ Next: [02 · What happens when you open an app?](../02-client-server-dns-http/README.md)

## In one line
**System design = deciding *which parts* a product needs and *how they connect*, so it keeps working well when many people use it.**

## Why do we need it?
An app that works for 10 users can completely fail for 10 lakh users:
- it becomes **slow**,
- it **crashes**,
- it **loses data** (orders disappear),
- it does **wrong things** (money deducted, no order).

The code may be correct. The **design** is what breaks at scale.

> Programming = building one good kitchen.
> System design = running a whole hotel chain.

## 🍽️ Our running example: Oota
Throughout this series we build **Oota**, a food delivery app.
- Day 1: one laptop, 10 friends. Works perfectly.
- Month 3: 1 lakh users at lunch time. Everything breaks.
Every video = one problem Oota faces + the concept that fixes it.

## ☕ Darshini analogy
| Darshini problem | Darshini fix | Tech name (coming soon) |
|---|---|---|
| 5,000 customers, 1 cook | Hire more cooks | More servers |
| Who goes to which cook? | Token counter | Load balancer |
| Same idli ordered again and again | Tray of ready idlis | Cache |
| People travel far to eat | Open branches in other areas | CDN |

## HLD vs LLD
| | HLD — High Level Design | LLD — Low Level Design |
|---|---|---|
| Question | Which big parts, and how do they talk? | What classes/objects inside one part? |
| Analogy | City plan | Floor plan of one building |
| Example | Servers, databases, caches, load balancers | Classes, interfaces, design patterns |
| Interview | "Design WhatsApp" | "Design a parking lot" |

## The 4 forces: S-S-R-C
Every design balances:
1. **Scale** – can it handle more users?
2. **Speed** – how fast does it respond?
3. **Reliability** – does it keep working when parts fail?
4. **Cost** – how much does it cost?

You can't maximise all four. Good design = the right **trade-off**.

> ⭐ **There is no best design. There is only the best design for *these* requirements.**

## Functional vs non-functional requirements
- **Functional** = what the system *does*: order food, track rider, pay online.
- **Non-functional** = how *well* it does it: menu loads in < 200 ms, never lose an order, handle 1 lakh orders at lunch.

Non-functional requirements decide most of the design.

## 🎯 Interview Line
**Q: What is the difference between HLD and LLD?**
> HLD defines the major components of a system and how they interact — services, databases, caches, load balancers. LLD goes inside one component and defines its classes, interfaces and design patterns.

## Key words from this video
`System` · `HLD` · `LLD` · `Scalability` · `Trade-off` · `Functional requirement` · `Non-functional requirement`
