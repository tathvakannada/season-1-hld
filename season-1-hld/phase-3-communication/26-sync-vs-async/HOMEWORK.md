# Homework · 26 · Sync vs Async communication

Post answers in **Discussions** (thread: *S1E26 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why must payment authorisation usually stay synchronous?
2. Async makes the *response* faster. Does it make the total work faster?

## ✏️ Draw
Draw Oota checkout as two lanes: "user waits" (sync) and "background" (async). Place all 8 steps.

## 🛠️ Build
Write a tiny Python script: a function that sleeps 2 s ("send SMS"). Call it directly vs push it to a `queue.Queue` handled by a worker thread. Compare response times.

## 🎯 Interview
"When would you use sync vs async? What are the downsides of async?" Compare with the [notes](README.md).
