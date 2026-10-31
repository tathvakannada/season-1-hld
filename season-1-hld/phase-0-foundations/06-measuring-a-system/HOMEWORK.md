# Homework · 06 · How do we measure a system?

Post answers in **Discussions** (thread: *S1E06 Homework*).

## 🧠 Think
1. Why does a company like Swiggy care more about p99 latency than the average?
2. Your system has an LB (99.99%), app (99.9%) and DB (99.9%) in series. What's the total availability? How would you improve it?

## ✏️ Draw
Draw where time is spent in **one** Oota "load menu" request: network, queue, code, cache/DB, network back. Guess a number (ms) for each piece.

## 🛠️ Build (10 minutes)
Open any website → **F12** → **Network** → refresh.
- Write down the time of 10 requests.
- Calculate the **average** and find the **slowest**. How different are they?

## 🎯 Interview
1. "99.99% availability means how much downtime per year?"
2. "Explain latency vs throughput with an example."
Compare with the [notes](README.md).
