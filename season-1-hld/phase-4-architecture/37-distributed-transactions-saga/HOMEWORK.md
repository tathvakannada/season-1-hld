# Homework · 37 · Distributed transactions: 2PC and Saga

Post answers in **Discussions** (thread: *S1E37 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why is 2PC called a blocking protocol?
2. Why should "send SMS" be the last step of a saga?

## ✏️ Draw
Draw the order saga's happy path left → right, and the failure path with compensations right → left.

## 🛠️ Build
Write a Python orchestrator: list of (step, compensation) functions; make step 3 fail and print the compensations running in reverse.

## 🎯 Interview
"2PC vs Saga? Choreography vs orchestration? What if a compensation fails?" Compare with the [notes](README.md).
