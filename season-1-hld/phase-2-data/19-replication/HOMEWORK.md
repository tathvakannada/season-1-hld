# Homework · 19 · Replication

Post answers in **Discussions** (thread: *S1E19 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why don't read replicas help with write load?
2. Why is a replica not a backup?

## ✏️ Draw
Draw a leader + 2 replicas. Show a user placing an order and immediately reading "My orders" from a lagging replica. Then draw the fix.

## 🛠️ Build
(Optional, Docker) Run a PostgreSQL primary + replica using a ready-made docker-compose example, insert on the primary, read on the replica.

## 🎯 Interview
"How does leader–follower replication work? Sync vs async — trade-offs?" Compare with the [notes](README.md).
