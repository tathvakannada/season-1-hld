# Homework · 51 · Design WhatsApp — Part 2: groups, presence, media, scale

Post answers in **Discussions** (thread: *S1E51 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why not send media through the WebSocket?
2. Why fetch "last seen" lazily instead of broadcasting it?

## ✏️ Draw
Draw a group message reaching 6 members on 3 gateways, 2 of them offline.

## 🛠️ Build
Redis: `SET presence:asha online EX 30`, then `TTL presence:asha` every few seconds. Watch Asha go "offline" when it expires.

## 🎯 Interview
"How would you add group chat and online status to your WhatsApp design?" Compare with the [notes](README.md).
