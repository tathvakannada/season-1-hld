# 31 · Real-time: polling, long polling, WebSockets, SSE

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [30](../../phase-3-communication/30-event-driven-architecture/README.md) · ⏭️ [32 · API design deep dive](../../phase-3-communication/32-api-design-deep-dive/README.md)

## The problem
Customers want the rider moving on the map and "Picked up!" instantly. But in HTTP the server can't start the conversation — it only answers.

## In one line
**To push live updates: ask repeatedly (polling), ask and wait (long polling), keep a two-way pipe (WebSockets), or a one-way stream (SSE).**

☕ **Darshini:** **polling** = asking "is my dosa ready?" every 30 s · **long polling** = "tell me when it's ready" and you wait at the counter · **WebSocket** = a walkie-talkie with the kitchen · **SSE** = the token display board · **push** = they phone you.

## The options
| | Direction | Latency | Server cost | Best for |
|---|---|---|---|---|
| **Short polling** | client asks every N s | up to N s | lots of wasted requests | simple, rare updates |
| **Long polling** | server answers when ready | low | holds connections | fallback |
| **WebSocket** | both ways | lowest | stateful connections | chat, games, collaboration |
| **SSE** | server → client | low | medium | status, live scores, AI token streaming |
| **Push (APNs/FCM)** | server → device | seconds | low | app closed |

Polling maths: 1 lakh users × every 2 s = **50,000 requests/s**, mostly "no update".

## WebSockets
Starts as HTTP with an **Upgrade**, becomes a persistent full-duplex connection.

## SSE
A normal HTTP request kept open; server streams events; browser auto-reconnects with the last event ID.

## Scaling WebSocket servers
- A user's socket lives on **one** gateway server
- Keep a **connection registry** (user → server) in Redis, or have gateways subscribe to **pub/sub** channels
- Least-connections load balancing · heartbeats · reconnect + resume

## Oota
Order status → SSE · rider live location → SSE/WebSocket every few seconds · chat with rider → WebSocket · app closed → push.

## 🎯 Interview Line
**Q: How would you show a rider's live location to the customer?**
> The rider app sends GPS every few seconds to a location service, which publishes updates on a pub/sub channel keyed by order. The gateway holding the customer's WebSocket or SSE connection pushes them to the app. Updates are throttled, history is stored asynchronously, and the app reconnects and resumes if the network drops.

## Key words
`Short polling` · `Long polling` · `WebSocket` · `SSE` · `Push notification` · `Connection registry` · `Heartbeat`
