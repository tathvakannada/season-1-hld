# Homework · 39 · Consensus, leader election & distributed locks

Post answers in **Discussions** (thread: *S1E39 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why use 3 or 5 nodes, not 4?
2. Why can a Redis lock with a TTL still let two workers act at once?

## ✏️ Draw
Draw 5 Raft nodes; kill the leader; show a candidate getting 3 votes.

## 🛠️ Build
Play with the interactive Raft visualisation at raft.github.io and stop/restart the leader.

## 🎯 Interview
"What is split brain and how do you prevent it? Explain Raft leader election." Compare with the [notes](README.md).
