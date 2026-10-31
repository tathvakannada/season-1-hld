# Homework · 28 · Publisher–Subscriber (Pub/Sub)

Post answers in **Discussions** (thread: *S1E28 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Queue or pub/sub: "send OTP SMS"? "rider assigned"?
2. Why is Redis Pub/Sub risky for important events?

## ✏️ Draw
Draw OrderPlaced fanning out to 5 services, then add a 6th without touching the order service.

## 🛠️ Build
Use `redis-cli`: open two terminals with `SUBSCRIBE orders`, then `PUBLISH orders "order 1023"` from a third. Close one subscriber and publish again — what did it miss?

## 🎯 Interview
"Queue vs pub/sub? Why publish events instead of calling services directly?" Compare with the [notes](README.md).
