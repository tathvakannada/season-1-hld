# Homework · 10 · Caching part 1

Post answers in **Discussions** (thread: *S1E10 Homework*).

## 🧠 Think
1. Why not cache *everything*?
2. Oota has 10 app servers, each with its own local cache. A restaurant changes its menu. What goes wrong?

## ✏️ Draw
Take the Oota diagram and mark **every place a cache could exist**, from the user's phone to the database. Write what you'd cache at each place.

## 🛠️ Build (15 minutes, needs Docker)
```
docker run -d -p 6379:6379 redis
docker exec -it <container> redis-cli
SET menu:42 "dose,idli,vada"
GET menu:42
EXPIRE menu:42 10
TTL menu:42
```
Wait 10 seconds and `GET menu:42` again. What happened?

## 🎯 Interview
"What is LRU and why is it common? How would you implement it in O(1)?"
