# Homework · 63 · Design a real-time leaderboard (Dream11)

Post answers in **Discussions** (thread: *S1E63 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why is the top 100 a perfect thing to cache?
2. Why compute final prize ranks from the DB, not Redis?

## ✏️ Draw
Draw the flow from "Kohli hits a six" to "my rank changes".

## 🛠️ Build
Redis: ZADD 10 players with scores, then ZREVRANGE 0 2 WITHSCORES and ZREVRANK for one player. ZINCRBY one and check again.

## 🎯 Interview
"Design a real-time leaderboard for 5 crore players." Compare with the [notes](README.md).
