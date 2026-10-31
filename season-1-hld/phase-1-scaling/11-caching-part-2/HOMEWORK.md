# Homework · 11 · Caching part 2

Post answers in **Discussions** (thread: *S1E11 Homework*).

## 🧠 Think
1. Which strategy would you use for a **bank balance**? For a **restaurant menu**? For a **like counter**? Why?
2. Why do we **delete** the cache key on update instead of **updating** it?

## ✏️ Draw
Draw the **cache-aside** flow twice: once for a read (hit and miss) and once for a write (update DB → delete cache).

## 🛠️ Build (20 minutes)
Implement cache-aside in ~20 lines of Python using a `dict` with expiry times:
- `get(key)` returns the value only if it hasn't expired
- `get_menu(id)` uses the cache, falls back to a fake "slow DB" function (`time.sleep(1)`)
- Measure the time of the first call vs the second call.

## 🎯 Interview
"How do you keep a cache and database consistent? What is a cache stampede and how do you prevent it?"
