# 18 · NoSQL family: key-value, document, wide-column, graph

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [17](../../phase-2-data/17-indexing/README.md) · ⏭️ [19 · Replication](../../phase-2-data/19-replication/README.md)

## The problem
Oota's data doesn't all fit neat tables: billions of tiny session lookups, menus with different shapes, huge GPS write streams, and "friends who ordered here" relationships.

## In one line
**NoSQL is four families, each built for a different data shape and access pattern.**

☕ **Darshini:** key-value = token number → parcel; document = each customer's full order slip; wide-column = the day's order log sorted by counter and time; graph = a map of who comes with whom.

## The 4 families
| Family | Data shape | Best at | Weak at | Examples | Oota use |
|---|---|---|---|---|---|
| **Key-value** | key → value | super-fast lookups by key | querying by value | Redis, DynamoDB | sessions, carts |
| **Document** | JSON documents | flexible objects read together | joins | MongoDB | menus / catalog |
| **Wide-column** | partition key + sort key | massive writes, time-ordered reads | ad-hoc queries | Cassandra, ScyllaDB | GPS history, events |
| **Graph** | nodes + edges | relationship traversal | horizontal scale | Neo4j | recommendations, fraud |

Also: **search engines** (Elasticsearch), **time-series** DBs (metrics), **vector** DBs (embeddings for AI search).

## Document modelling: embed or reference?
- **Embed** small data that's read together (menu inside restaurant)
- **Reference** data that grows forever or is shared (orders)

## Wide-column modelling
Design tables **around your queries** ("events for rider R between T1 and T2"). Data is often duplicated, one table per query.

## Schema
SQL = schema-on-write (DB enforces). Most NoSQL = schema-on-read (your code enforces).

## 🎯 Interview Line
**Q: When would you choose Cassandra over PostgreSQL?**
> When we need very high write throughput, data naturally partitioned by a key, simple query patterns known in advance, and easy horizontal scaling with high availability — and we can accept eventual consistency and query-first data modelling.

## Key words
`Key-value` · `Document` · `Wide-column` · `Graph` · `Partition key` · `Embed vs reference` · `Schema-on-read`
