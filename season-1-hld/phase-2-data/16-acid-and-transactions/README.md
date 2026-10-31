# 16 · SQL deep dive: ACID & transactions

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [15](../../phase-1-scaling/15-phase-1-recap/README.md) · ⏭️ [17 · Indexing: finding one row in crores](../../phase-2-data/17-indexing/README.md)

## The problem
Placing an order = deduct wallet + create order + book delivery slot. Crash after step 1 → money gone, no order. Two users book the **last** slot at the same millisecond → both succeed. 😱

## In one line
**A transaction groups changes into one all-or-nothing unit; ACID makes it trustworthy.**

☕ **Darshini:** the cashier takes your money **and** gives your token together — if the printer jams, you get your money back.

## Transactions
```sql
BEGIN;
  UPDATE wallets SET balance = balance - 240 WHERE user_id = 42 AND balance >= 240;
  INSERT INTO orders (user_id, restaurant_id, total) VALUES (42, 7, 240);
  UPDATE slots SET booked = booked + 1 WHERE slot_id = 9 AND booked < capacity;
COMMIT;   -- or ROLLBACK
```

## ACID
| Letter | Meaning | Oota example |
|---|---|---|
| **A**tomicity | all or nothing | debit + order happen together or not at all |
| **C**onsistency | data rules never broken | balance never below 0, order must have a real user |
| **I**solation | concurrent transactions don't corrupt each other | two checkouts don't mess up one wallet |
| **D**urability | committed = saved, even after a crash | your order survives a power cut |

⚠️ The **C in ACID ≠ the C in CAP** (that one is about replicas agreeing).

## Isolation problems
| Anomaly | What happens |
|---|---|
| Dirty read | reading data that isn't committed yet |
| Non-repeatable read | same row read twice, different values |
| Phantom read | same query, new rows appear |
| Lost update | two read-modify-writes overwrite each other |
| Write skew | two transactions check a rule, both write, rule broken |

Isolation levels: Read Uncommitted → **Read Committed** (PostgreSQL default) → **Repeatable Read** (MySQL default) → Serializable (safest, slowest).

## Handling concurrency
- **Atomic updates:** `SET balance = balance - 240 WHERE balance >= 240`
- **Pessimistic locking:** `SELECT … FOR UPDATE` — lock first, others wait
- **Optimistic locking:** `version` column — update only if unchanged, else retry
- **Constraints:** unique index (one booking per seat), `CHECK (balance >= 0)`

## When do you need ACID?
Money, orders, inventory, bookings → yes. Likes, views, analytics → usually no.

## 🎯 Interview Line
**Q: Explain ACID with an example.**
> Placing an order: atomic — wallet debit and order creation happen together or not at all; consistent — rules like 'balance ≥ 0' always hold; isolated — two simultaneous checkouts don't corrupt the balance; durable — once committed, the order survives crashes.

## Key words
`Transaction` · `ACID` · `Rollback` · `Isolation level` · `Lost update` · `Optimistic locking` · `Pessimistic locking` · `WAL`
