# 05 · Databases 101 (why a DB, tables vs documents, SQL vs NoSQL)

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [04](../04-apis-rest/README.md) · ⏭️ [06 · Measuring a system](../06-measuring-a-system/README.md)

## In one line
**A database stores data permanently and lets many users read and write it quickly and safely at the same time.**

## Why not just a file?
Oota saved orders in `orders.txt`:
- 👥 Two writes at the same second → **one is lost**
- 💥 Crash halfway → **half an order** saved
- 🐢 "Find user 42's orders" → **read the whole file**
- 🚫 No rules → an order for an item that **doesn't exist**

## What a database gives you
1. **Persistence** — survives restarts and crashes
2. **Fast queries** — indexes (Phase 2)
3. **Concurrency** — many users at once, safely
4. **Integrity** — rules keep data correct
5. **Transactions** — all or nothing (Phase 2)

☕ Darshini: loose papers on the counter vs the manager's **register** in the store room.

## Relational (SQL) databases
- Data in **tables** (rows + columns) with a fixed **schema**.
- **Primary key (PK)** — unique ID of a row (like an Aadhaar number for the row).
- **Foreign key (FK)** — points to a row in another table (`orders.user_id → users.id`).

```
users(id PK, name, phone, city)
restaurants(id PK, name, city, rating)
orders(id PK, user_id FK, restaurant_id FK, status, total, created_at)
```
```sql
SELECT id, status, total FROM orders WHERE user_id = 42 ORDER BY created_at DESC;
INSERT INTO orders (user_id, restaurant_id, status, total) VALUES (42, 7, 'PLACED', 240);
```
Examples: PostgreSQL, MySQL, Oracle, SQL Server.

## NoSQL — a first look
- **Key-value** (Redis, DynamoDB): `session:abc123 → {user_id: 42}`. A giant dictionary; super fast lookups by key.
- **Document** (MongoDB): JSON-like documents; each can have a different shape.
(Wide-column and graph databases come in Phase 2.)

## SQL vs NoSQL
| | SQL | NoSQL |
|---|---|---|
| Data shape | Fixed tables | Flexible |
| Relationships | Excellent (JOINs) | Weak; data often duplicated |
| Transactions | Strong, built in | Varies |
| Scaling | Scale up first; sharding is harder | Many designed to scale out |
| Good for | Orders, payments | Sessions, catalogs, huge write volume |

**Rule of thumb:** start with SQL unless you have a clear reason not to. Real companies use **several databases together** (Oota: SQL for orders, Redis for sessions, documents for the catalog). Choose by **how you access the data**.

## Where the DB sits
`App → Backend → Database` — apps **never** connect to the DB directly. The backend keeps a **connection pool** (reuses connections; opening one per request is slow).

## ⚖️ 3 myths
- "NoSQL is always faster" ❌ — depends on the access pattern.
- "NoSQL has no schema" ❌ — the schema moves into your code.
- "SQL doesn't scale" ❌ — it does; scaling writes just takes more work.

## 🎯 Interview Line
**Q: SQL vs NoSQL — how do you choose?**
> Based on the data model, access patterns, consistency needs and scale. Relational data needing transactions (orders, payments) → SQL. Flexible schemas, simple key lookups at very large scale, or very high write throughput → the right type of NoSQL. Many systems use both.

## Key words
`Database` · `Table` · `Schema` · `Primary key` · `Foreign key` · `SQL` · `JOIN` · `NoSQL` · `Key-value` · `Document` · `Connection pool`
