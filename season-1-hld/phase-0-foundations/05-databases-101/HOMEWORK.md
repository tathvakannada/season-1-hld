# Homework · 05 · Databases 101

Post answers in **Discussions** (thread: *S1E05 Homework*).

## 🧠 Think
1. List 4 things that can go wrong if Oota stores orders in a text file.
2. Why does a foreign key make data safer?

## ✏️ Draw
Draw Oota's 4 tables — `users`, `restaurants`, `menu_items`, `orders` — with their columns. Mark every **PK** and **FK** and draw arrows for the relationships.
Bonus: an order can have many items. Which extra table do you need? (Hint: `order_items`.)

## 🛠️ Build (15 minutes, no installation)
Open any free online SQL playground (search "SQLite online" or use sqlbolt.com for practice). Then:
```sql
CREATE TABLE orders (id INTEGER PRIMARY KEY, user_id INTEGER, total INTEGER, status TEXT);
INSERT INTO orders (user_id, total, status) VALUES (42, 240, 'PLACED');
INSERT INTO orders (user_id, total, status) VALUES (42, 180, 'DELIVERED');
INSERT INTO orders (user_id, total, status) VALUES (7, 90, 'PLACED');
SELECT * FROM orders WHERE user_id = 42;
```
Now write a query to find all orders with status `PLACED`.

## 🎯 Interview
"SQL vs NoSQL — how do you choose?" Answer in 4 lines with one example each. Compare with the [notes](README.md).
