# Homework · 07 · Vertical vs Horizontal Scaling

Post answers in **Discussions** (thread: *S1E07 Homework*).

## 🧠 Think
1. Name 2 hidden costs of horizontal scaling.
2. Why is scaling the database horizontally much harder than scaling app servers?

## ✏️ Draw
Draw Oota with **3 app servers**. Users only know one address. What box is missing in front of the servers? Draw it with a "?".

## 🛠️ Build (15 minutes)
Run the same tiny web app on **two ports** on your laptop. Example with Python:
```
python -m http.server 8001
python -m http.server 8002
```
Open both in the browser. You now have "2 servers" — next video we'll put a load balancer in front.

## 🎯 Interview
"Vertical vs horizontal scaling — what would you do first for a startup, and why?"
