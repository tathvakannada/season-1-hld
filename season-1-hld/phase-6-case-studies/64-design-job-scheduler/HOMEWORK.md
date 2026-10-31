# Homework · 64 · Design a distributed job scheduler

Post answers in **Discussions** (thread: *S1E64 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why is a single cron server a problem?
2. Why add jitter to non-critical jobs scheduled at midnight?

## ✏️ Draw
Draw 3 schedulers owning 9 buckets; one dies — show who takes over its buckets.

## 🛠️ Build
Redis sorted set as a scheduler: ZADD jobs <timestamp> job_id; a Python loop that ZRANGEBYSCORE jobs 0 now, runs and removes them.

## 🎯 Interview
"Design a distributed cron/job scheduler." Compare with the [notes](README.md).
