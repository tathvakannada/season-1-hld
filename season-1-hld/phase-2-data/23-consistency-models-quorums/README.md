# 23 · Consistency models & quorums

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [22](../../phase-2-data/22-cap-theorem/README.md) · ⏭️ [24 · Storage types: block, file, object](../../phase-2-data/24-storage-types/README.md)

## The problem
After updating my address, do **I** see it? Can my order status go backwards on refresh? With 3 replicas, how many must confirm a write?

## In one line
**A consistency model is the system's promise about what a read returns after a write; quorums (R + W > N) give stronger reads.**

☕ **Darshini:** 3 notice boards. A new price is pinned on 2 boards; a customer checks 2 boards — at least one has the new price.

## The spectrum (strong → weak)
1. **Strong (linearizable):** everyone sees the write immediately
2. **Causal:** cause before effect (a reply never appears before its question)
3. **Session guarantees:** **read-your-writes**, **monotonic reads** (never go back in time)
4. **Eventual:** replicas converge *eventually*

| Oota feature | Needs |
|---|---|
| Wallet, payment status | strong |
| "My orders" right after ordering | read-your-writes |
| Order status timeline | monotonic reads |
| Chat with restaurant | causal |
| Ratings, likes, menus | eventual |

## Quorums: R + W > N
N replicas · write confirmed by **W** · read from **R** · if **R + W > N**, read and write sets overlap → read sees the latest write.

| N=3 | W | R | Overlap? |
|---|---|---|---|
| balanced | 2 | 2 | ✅ |
| fast reads | 3 | 1 | ✅ |
| fast writes | 1 | 3 | ✅ |
| fastest, eventual | 1 | 1 | ❌ |

## How replicas catch up
Read repair · anti-entropy background sync · conflict resolution (last-write-wins, version vectors, CRDTs).

## 🎯 Interview Line
**Q: Explain R + W > N.**
> Each item is stored on N replicas. A write succeeds after W acknowledge and a read queries R. If R + W > N, the read and write sets overlap in at least one replica, so the read sees the latest successful write. With N = 3, W = 2 and R = 2 is a common balanced choice.

## Key words
`Strong consistency` · `Eventual consistency` · `Read-your-writes` · `Monotonic reads` · `Causal` · `Quorum` · `N/W/R` · `Read repair`
