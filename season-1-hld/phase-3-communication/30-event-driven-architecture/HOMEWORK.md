# Homework · 30 · Event-driven architecture (event sourcing, CQRS, outbox)

Post answers in **Discussions** (thread: *S1E30 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why is event sourcing like a bank passbook?
2. What goes wrong if you update the DB and then publish to Kafka without an outbox?

## ✏️ Draw
Draw an order's events as a timeline, then draw two read models built from them (customer timeline + ops dashboard).

## 🛠️ Build
In Python, store a list of order events and write a `replay(events)` function that returns the current order state.

## 🎯 Interview
"What is event sourcing? What is CQRS? How does the outbox pattern work?" Compare with the [notes](README.md).
