# Homework · 35 · Microservice patterns: discovery, timeouts, retries, circuit breaker, bulkhead

Post answers in **Discussions** (thread: *S1E35 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why add jitter to retries?
2. A circuit breaker is OPEN. Is the dependency healthy or broken?

## ✏️ Draw
Draw the cascading failure (coupon → order → gateway), then the same picture with the 4 protections.

## 🛠️ Build
Write a tiny Python circuit breaker class (closed/open/half-open, failure threshold, cool-down) and test it with a fake flaky function.

## 🎯 Interview
"Explain the circuit breaker pattern. How do you prevent cascading failures?" Compare with the [notes](README.md).
