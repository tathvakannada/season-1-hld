# Homework · 36 · Rate Limiting

Post answers in **Discussions** (thread: *S1E36 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why is rate limiting only by IP not enough?
2. Show how a fixed window of 100/min can allow 200 requests in 2 seconds.

## ✏️ Draw
Animate a token bucket on paper: capacity 5, refill 1/second, 8 requests arrive in 1 second.

## 🛠️ Build
Implement a token bucket in Python (or in Redis with INCR/EXPIRE) and hammer it with a loop.

## 🎯 Interview
"Design a rate limiter for Oota's OTP API." Compare with the [notes](README.md).
