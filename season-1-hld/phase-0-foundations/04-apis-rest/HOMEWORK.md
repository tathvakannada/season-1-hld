# Homework · 04 · APIs explained

Post answers in **Discussions** (thread: *S1E04 Homework*).

## 🧠 Think
1. Why should a `GET` request never change data?
2. Why is it dangerous for a mobile app to connect directly to the database?

## ✏️ Draw / Design
Design **3 APIs for the Oota cart**: view cart, add an item, remove an item. For each, write: method + URL + example request body + success status code.

## 🛠️ Build (10 minutes)
Call a free public JSON API — for example `https://jsonplaceholder.typicode.com/posts/1` — using your browser, Postman, or:
```
curl -i https://jsonplaceholder.typicode.com/posts/1
```
- What status code did you get? Which headers do you see?
- Now try an ID that doesn't exist (e.g. `/posts/99999`). What changes?

## 🎯 Interview
1. "PUT vs PATCH vs POST?"
2. "401 vs 403?"
Compare with the [notes](README.md).
