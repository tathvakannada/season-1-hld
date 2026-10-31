# Homework · 21 · Consistent Hashing

Post answers in **Discussions** (thread: *S1E21 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why do we need virtual nodes?
2. Does consistent hashing solve a hot-key problem? Why not?

## ✏️ Draw
Draw a ring with 4 servers and 8 keys. Add a 5th server and mark which keys move.

## 🛠️ Build
Implement a tiny hash ring in Python (sorted list + `bisect`), add 100 vnodes per server, and measure the % of keys moved when adding a server.

## 🎯 Interview
"Why not hash % N? Explain consistent hashing and virtual nodes." Compare with the [notes](README.md).
