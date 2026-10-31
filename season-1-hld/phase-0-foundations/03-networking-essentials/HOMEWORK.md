# Homework · 03 · Networking you actually need

Post answers in **Discussions** (thread: *S1E03 Homework*).

## 🧠 Think
1. Why does a video call prefer UDP? Why must a payment use TCP?
2. Why do companies keep the database in a *private* network?
3. If UDP is "unreliable", why is HTTP/3 built on it?

## ✏️ Draw
Draw the **TCP handshake** as a conversation between two people (like a comic strip). Then add a 4th panel where the real data is sent.

## 🛠️ Build (10 minutes)
On Windows (Command Prompt) — use `traceroute` instead of `tracert` on Mac/Linux:
```
ping google.com
tracert google.com
```
- What is the average time (ms) in `ping`?
- How many hops does `tracert` show? Can you guess which cities some hops are in from their names?
- Try `ping` to a website hosted far away and compare the time.

## 🎯 Interview
**"TCP vs UDP — when would you use each?"** Answer in 3 lines with one example each. Compare with the *Interview Line* in the [notes](README.md).
