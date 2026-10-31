# Homework · 48 · Design a distributed rate limiter

Post answers in **Discussions** (thread: *S1E48 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why is a per-server in-memory counter not enough?
2. Should a payment API fail open or fail closed? Why?

## ✏️ Draw
Draw the race condition (two gateways, one counter) and then the fix with Lua.

## 🛠️ Build
Run Redis in Docker and write a token-bucket Lua script; call it from Python 20 times in a loop and print allowed/denied.

## 🎯 Interview
"Design a rate limiter for an API used by 1 crore users." Compare with the [notes](README.md).
