# 51 · Design WhatsApp — Part 2: groups, presence, media, scale

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [50](../../phase-6-case-studies/50-design-chat-part-1/README.md) · ⏭️ [52 · Design a news feed (Instagram)](../../phase-6-case-studies/52-design-news-feed/README.md)

## The problem
Part 1 handled Asha → Ravi. Now: a 500-member family group, "online" and "typing…", a 50 MB video forward, WhatsApp Web + phone together, and 20 crore open connections.

## In one line
**Fan out group messages per member (batched per gateway), keep presence as heartbeat keys with TTL, send media through object storage with only a pointer in the chat, and sync every device with its own cursor.**

☕ **Darshini:** for a 500-person wedding, the cook doesn't take 500 separate orders — one list per table (gateway), and the big video is a parcel kept at the counter with just its token number sent around.

## Groups
- One message stored once → delivered to every member (online → their gateway; offline → inbox + push)
- **Batch per gateway** (one hop per server, not per member)
- Small groups: fan-out on write · huge broadcast channels: fan-out on read
- Read receipts: per-member "last read message" pointer

## Presence & typing
- Gateway heartbeats → Redis `presence:user` with **TTL** (expires → offline, last seen)
- Fetch presence **lazily** when a chat is open — don't broadcast to all contacts
- Typing = ephemeral, never stored

## Media
```
App → presigned URL → Object storage (+CDN)
App → chat message with { media_key, thumbnail, hash }
```
Dedupe forwarded viral videos by hash

## Multi-device
Each device: own connection + own sync cursor · deliver to **all** devices

## Scaling connections
~2,000+ gateways tuned for idle sockets · registry with heartbeats · reconnect with **jittered backoff** after a crash · nearest region

## Architecture
```
Apps ══WS══► Gateways ◄─► Registry + Presence (Redis)
                 ▼
            Chat service → Message store (chat_id + time bucket)
                 ├─► Group service (members, cached)
                 ├─► per-gateway delivery topics
                 └─► Notification service (offline push)
```

## 🎯 Interview Line
**Q: How do you deliver a message to a 500-member group efficiently?**
> Store the message once, look up members from a cached group service, group the online members by the gateway they're connected to and send one batch per gateway; offline members get it in their pending inbox plus one collapsed push notification. For huge broadcast channels I'd switch to fan-out on read.

## Key words
`Group fan-out` · `Presence` · `Heartbeat` · `TTL` · `Presigned URL` · `Multi-device sync` · `Jittered backoff`
