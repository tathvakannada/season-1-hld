# 58 · Design file storage & sync (Google Drive)

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [57](../../phase-6-case-studies/57-design-ticket-booking/README.md) · ⏭️ [59 · Design search autocomplete](../../phase-6-case-studies/59-design-search-autocomplete/README.md)

## The problem
You edit 1 line in a 1 GB file on your laptop. It should appear on your phone in seconds — without uploading 1 GB again — and two offline edits must never silently overwrite each other.

## In one line
**Split files into content-hashed blocks, upload only the blocks the server doesn't have, keep a metadata journal of changes, and let every device pull changes since its last cursor.**

☕ **Darshini:** a recipe book kept as numbered pages: change one page and you only reprint that page, and every branch checks the "changes since page list #42" register.

## R · Requirements
Upload/download big files · sync across devices · offline edits · share · versions
**Durability above all** · bandwidth-efficient · conflicts handled

## E · Estimation
10 crore users × 10 GB ≈ **1 EB** → object storage · metadata operations ≫ file transfers

## A · API
`POST /files/uploads {block_hashes}` → missing blocks · `POST …/commit` · `GET /changes?cursor=` · `POST /files/{id}/share`

## D · Data
**Blocks** (4 MB, by SHA-256) in object storage · **Metadata** (SQL): files, versions (list of block hashes), change journal, shares

## H · High-level design
```
Client sync agent (watch folder, chunk, hash)
  ├─► Metadata svc (SQL) — commit versions, change journal
  ├─► Block svc → Object storage (presigned, dedupe by hash)
  └─◄ Notification: "something changed" → GET /changes?cursor
```

## D · Deep dives
- **Delta sync:** small edit → re-upload only changed blocks
- **Dedupe:** same block hash stored once
- **Sync:** ordered change journal + cursor per device; notify cheap, pull details
- **Conflicts:** commit based on an old version → keep both as "conflicted copy"
- **Durability:** replication/erasure coding, version history, trash, backups

## 🎯 Interview Line
**Q: How do you avoid re-uploading a whole file after a small change?**
> Split files into blocks and identify each by its content hash. The client sends the list of hashes; the server replies with the ones it doesn't already have, and only those blocks are uploaded. A new version is just a new list of block hashes in the metadata database.

## Key words
`Chunking` · `Content hash` · `Deduplication` · `Delta sync` · `Change journal` · `Cursor` · `Conflicted copy` · `Object storage`
