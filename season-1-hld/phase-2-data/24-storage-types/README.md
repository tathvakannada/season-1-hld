# 24 · Storage types: block, file, object

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [23](../../phase-2-data/23-consistency-models-quorums/README.md) · ⏭️ [25 · How to choose a database + Phase 2 recap](../../phase-2-data/25-choosing-a-database/README.md)

## The problem
Food photos stored inside the database → DB is 90% photos: slow backups, lagging replicas, wasted memory.

## In one line
**Big files go to object storage (like S3); the database stores only the file's key; a CDN delivers it.**

☕ **Darshini:** the **register** (DB) only writes "photo is in the godown, shelf 42"; the sacks (files) sit in the **godown** (object storage).

## Three kinds of storage
| | Block | File | Object |
|---|---|---|---|
| What | raw disk volume | shared folders | flat bucket of objects via HTTP |
| Used by | one server | many servers mount it | anyone with permission |
| Speed | fastest | medium | higher latency, huge throughput |
| Scale | one volume | large | practically unlimited |
| Examples | AWS EBS, SSD | AWS EFS, NFS | **AWS S3**, GCS, Azure Blob |
| Good for | DB files, OS disks | shared files | images, videos, backups, logs |

## Object storage
- `key → object` (`restaurants/42/dose.jpg`); "folders" are just prefixes
- Very high durability (S3 is designed for eleven nines)
- Storage classes: hot vs cold/archive; lifecycle rules
- Events: "new upload" → trigger thumbnail job

## The standard upload pattern
1. App asks backend: "I want to upload"
2. Backend returns a **presigned URL** (short-lived permission)
3. App **uploads directly** to object storage
4. Backend saves only the **key** in the DB
5. Users view it through the **CDN**

> 💡 "Files in the bucket, pointers in the DB, CDN in front."

## 🎯 Interview Line
**Q: Where would you store user-uploaded photos?**
> In object storage like S3 — it's durable, cheap and practically unlimited. The app uploads directly using a presigned URL, the database stores only the object key and metadata, and a CDN serves the images to users.

## Key words
`Block storage` · `File storage` · `Object storage` · `Bucket` · `Presigned URL` · `Storage class` · `Data lake`
