# 22 · CAP theorem (and PACELC)

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [21](../../phase-2-data/21-consistent-hashing/README.md) · ⏭️ [23 · Consistency models & quorums](../../phase-2-data/23-consistency-models-quorums/README.md)

## The problem
Mumbai and Chennai data centres lose the link between them. A user updates her address in Chennai; Mumbai is asked for it. Old answer, or no answer?

## In one line
**During a network partition, a distributed system must choose Consistency or Availability.**

☕ **Darshini:** two branches, phone line cut. **CP branch:** "Can't confirm your balance, come later." **AP branch:** "Here's the balance we last knew" (might be old).

## C, A, P
- **Consistency:** every read sees the latest write (acts like one copy)
- **Availability:** every request to a working node gets a response
- **Partition tolerance:** keeps working when the network between nodes breaks

## The real statement
Partitions **will** happen, so P isn't optional. **During a partition: choose C or A.**
- **CP:** refuse/timeout rather than return stale data
- **AP:** keep answering, reconcile later
"Pick any two" is misleading — CA only exists on a single machine.

## Typical examples (often tunable)
| Leans CP | Leans AP |
|---|---|
| ZooKeeper, etcd, HBase, MongoDB (majority) | Cassandra, DynamoDB (default), Riak |

## Choose per feature
- **C:** payments, wallet, seat booking, inventory
- **A:** menus, ratings, likes, feeds, rider location

## PACELC
**If Partition → A or C; Else → Latency or Consistency.**
Even with a healthy network, strong consistency costs latency (waiting for replicas).

## 🎯 Interview Line
**Q: Explain the CAP theorem.**
> In a distributed system, network partitions are unavoidable, so when one happens we must choose: consistency — nodes refuse requests rather than return stale data — or availability — every node keeps responding, possibly with stale data. I'd choose consistency for payments and bookings, availability for feeds and menus.

## Key words
`Consistency` · `Availability` · `Partition tolerance` · `CP` · `AP` · `PACELC`
