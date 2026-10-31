# 52 · Design a news feed (Instagram)

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [51](../../phase-6-case-studies/51-design-chat-part-2/README.md) · ⏭️ [53 · Design YouTube — Part 1: upload, transcoding, playback](../../phase-6-case-studies/53-design-youtube-part-1/README.md)

## The problem
50 crore people open their feed 20 times a day and expect it in 200 ms. When a celebrity with 5 crore followers posts, do we really write 5 crore times?

## In one line
**Precompute feeds for normal users (fan-out on write), pull celebrity posts at read time (fan-out on read), store only post IDs in the feed cache and hydrate them when reading.**

☕ **Darshini:** regular customers get their usual coffee kept ready at their table (push); the celebrity chef's special is only fetched from the kitchen when someone asks (pull).

## R · Requirements
Post · follow · home feed (newest/ranked, infinite scroll) · fast reads (<200 ms), slight staleness OK, celebrities

## E · Estimation
~500 posts/s · **~10⁵ feed reads/s** · read:write ≈ 200:1 → precompute + cache

## A · API
`POST /v1/posts` · `POST /v1/users/{id}/follow` · `GET /v1/feed?cursor=&limit=20`

## D · Data
posts (Snowflake IDs) · follows (both directions) · **feed_cache: user → list of post IDs** (Redis, capped)

## H · High-level design
```
Post → Post svc → DB + media(CDN) → Kafka → Fan-out workers → feed_cache[each follower]
Feed → Feed svc → feed_cache[me] → hydrate (posts, authors, counts) → rank → page
```

## D · Deep dives
| | Push (write) | Pull (read) |
|---|---|---|
| Read speed | ⚡ fast | 🐢 slow |
| Celebrity post | 5 crore writes 😱 | fine |
**Hybrid:** push for normal users, pull for celebrities, skip inactive users
- Feed cache holds **IDs** → deletes/edits handled at hydration
- Cursor pagination · ranking as a separate service

## 🎯 Interview Line
**Q: How do you handle celebrities in a news feed?**
> Use a hybrid: for normal users, fan out each new post ID into followers' precomputed feeds; for accounts above a follower threshold, skip fan-out and instead merge their recent posts into the feed at read time. That avoids crores of writes per celebrity post while keeping reads fast.

## Key words
`Fan-out on write` · `Fan-out on read` · `Hybrid fan-out` · `Celebrity problem` · `Hydration` · `Cursor pagination`
