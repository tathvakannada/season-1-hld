# Homework · 20 · Sharding (Partitioning)

Post answers in **Discussions** (thread: *S1E20 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why is `created_at` a bad shard key for range sharding?
2. Oota is sharded by user_id. How would you show "all orders of restaurant 7"?

## ✏️ Draw
Draw 4 shards using hash(user_id) % 4. Place users 1–12. Then change to % 5 and count how many users moved.

## 🛠️ Build
Write a 10-line Python script: hash 10,000 keys with `% 4` and `% 5` and print the % of keys that moved.

## 🎯 Interview
"Range vs hash sharding? What problems does sharding introduce?" Compare with the [notes](README.md).
