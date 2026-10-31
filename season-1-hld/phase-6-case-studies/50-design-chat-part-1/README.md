# 50 · Design WhatsApp — Part 1: 1:1 messaging

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [49](../../phase-6-case-studies/49-design-notification-system/README.md) · ⏭️ [51 · Design WhatsApp — Part 2: groups, presence, media, scale](../../phase-6-case-studies/51-design-chat-part-2/README.md)

## The problem
Asha sends "Oota ge banni!" to Ravi. It must reach in under half a second if he's online, wait safely if he's offline, never get lost, never appear twice, and show ✓, ✓✓ and blue ✓✓.

## In one line
**Phones keep a WebSocket to a gateway; the chat service persists each message, acks the sender, then routes it to the recipient's gateway — or stores it and sends a push if they're offline.**

☕ **Darshini:** a parcel counter: the clerk writes your parcel in the register (✓), hands it to the delivery boy who gets a signature (✓✓), and the receiver opens it (blue ✓✓).

## R · Requirements
1:1 messages · receipts · offline delivery · history · **never lost, ordered per chat, no duplicates**, real time

## E · Estimation
50 crore DAU × 40 msgs → **~2 lakh msgs/s** · ~20 crore live connections ÷ 1 lakh per server ≈ **2,000 gateways** · 2 TB text/day

## A · Protocol (WebSocket events)
`send_message{client_msg_id, chat_id, body}` · `message.ack` · `receipt.delivered` · `receipt.read`

## D · Data model
`messages(chat_id, message_id, sender, body, ts)` — partition **chat_id**, sort **message_id** → wide-column store · connection registry (Redis): user → gateway

## H · High-level design
```
Asha ══WS══► Gateway A → Chat service: persist → ack Asha (✓)
                         → registry lookup → Gateway B ══► Ravi (acks → ✓✓)
                         → offline? pending inbox + push notification
```

## D · Deep dives
| Tick | Meaning |
|---|---|
| ✓ | server **saved** it |
| ✓✓ | Ravi's device **received** it |
| blue ✓✓ | Ravi **read** it |
- **client_msg_id** from the phone → retries don't duplicate
- **Order per chat** (server sequence), not by phone clocks
- Offline sync: "give me everything after message X"

## 🎯 Interview Line
**Q: How does WhatsApp know which server to send Ravi's message to?**
> Each gateway records active connections in a connection registry — a Redis map of user/device to gateway with heartbeat TTLs. The chat service looks Ravi up there and routes the message to that gateway; if there's no entry, Ravi is offline, so the message waits in his pending inbox and a push notification is sent.

## Key words
`WebSocket` · `Gateway` · `Connection registry` · `client_msg_id` · `Delivery receipt` · `Wide-column store` · `Store-and-forward`
