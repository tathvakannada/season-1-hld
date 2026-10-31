# Homework · 23 · Consistency models & quorums

Post answers in **Discussions** (thread: *S1E23 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. With N=3, why is W=1, R=1 fast but risky?
2. Which guarantee stops an order status going from Delivered back to Out for delivery?

## ✏️ Draw
Draw 3 replica boxes. Colour the W=2 written replicas, circle the R=2 read replicas, and show the overlap.

## 🛠️ Build
Write a tiny Python simulation of 3 replicas with versions; try (W,R) = (1,1), (2,2), (3,1) and see when reads return stale data.

## 🎯 Interview
"Strong vs eventual consistency — with examples. What is read-your-writes and how would you implement it?" Compare with the [notes](README.md).
