# Homework · 16 · SQL deep dive: ACID & transactions

Post answers in **Discussions** (thread: *S1E16 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why is `balance = read(); write(balance - 240)` in app code dangerous?
2. When is optimistic locking better than pessimistic?

## ✏️ Draw
Draw two users booking the **last delivery slot** at the same time as a timeline (two columns). Show how `WHERE booked < capacity` saves you.

## 🛠️ Build
In an online PostgreSQL playground, create `wallets(user_id, balance)`, run a transaction that debits ₹240, then try one that would go negative with `CHECK (balance >= 0)` — see the ROLLBACK.

## 🎯 Interview
"Explain ACID with an example. What is a lost update and how do you prevent it?" Compare with the [notes](README.md).
