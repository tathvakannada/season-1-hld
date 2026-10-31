# Homework · 17 · Indexing: finding one row in crores

Post answers in **Discussions** (thread: *S1E17 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why is an index on `is_veg` almost useless?
2. Index on `(city, rating)`: which of these queries use it — by city; by rating; by city sorted by rating?

## ✏️ Draw
Draw a small 3-level B-tree for user IDs 1–100 and trace how you find user 42.

## 🛠️ Build
In SQLite/PostgreSQL: create a table with 1 lakh rows (use a loop or `generate_series`), time `SELECT … WHERE user_id = 42`, add an index, time it again. Run `EXPLAIN` before and after.

## 🎯 Interview
"How does an index work? Why not index every column?" Compare with the [notes](README.md).
