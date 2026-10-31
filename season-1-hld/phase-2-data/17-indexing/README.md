# 17 · Indexing: finding one row in crores

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [16](../../phase-2-data/16-acid-and-transactions/README.md) · ⏭️ [18 · NoSQL family: key-value, document, wide-column, graph](../../phase-2-data/18-nosql-family/README.md)

## The problem
`orders` has 50 crore rows. "Show user 42's orders" takes 9 seconds — the DB reads **every row** (full table scan).

## In one line
**An index is an extra sorted structure that lets the DB jump to matching rows — fast reads, slower writes.**

☕ **Darshini:** the **index page at the back of a textbook** vs flipping through every page.

## Without vs with an index
- Full table scan: O(n) — read everything
- Index lookup: O(log n) — a few hops down a tree

## B+ tree (the default)
Wide, shallow, **sorted** tree — even crores of rows need only ~4 levels. Leaves are linked in order → great for **ranges** and **ORDER BY**.

## Other index types
- **Hash:** exact match only
- **LSM tree:** write-heavy stores (Cassandra, RocksDB)
- **Full-text (inverted):** search boxes (Elasticsearch)
- **Geospatial:** "restaurants near me"

## Composite indexes
Index on `(user_id, created_at)`:
- ✅ `WHERE user_id = 42`
- ✅ `WHERE user_id = 42 ORDER BY created_at DESC`
- ❌ `WHERE created_at > X` alone — not the **leftmost** column

**Covering index:** contains all columns the query needs → answered from the index alone.

## The cost
- Every write updates every index → **slower writes**
- Extra storage and memory
- Low-selectivity columns (`is_veg`) → nearly useless index
➡️ Index **for your real queries**. Check with `EXPLAIN ANALYZE`.

## 🎯 Interview Line
**Q: How does an index speed up a query, and what does it cost?**
> An index is a sorted structure, usually a B+ tree, so the database finds matching rows in O(log n) instead of scanning the whole table. The cost is slower writes, because every index must be updated, plus extra storage and memory.

## Key words
`Full table scan` · `B+ tree` · `Composite index` · `Leftmost prefix` · `Covering index` · `Selectivity` · `LSM tree` · `EXPLAIN`
