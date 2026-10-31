# 49 · Design a notification system

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [48](../../phase-6-case-studies/48-design-distributed-rate-limiter/README.md) · ⏭️ [50 · Design WhatsApp — Part 1: 1:1 messaging](../../phase-6-case-studies/50-design-chat-part-1/README.md)

## The problem
Oota sends OTPs, "rider is 2 minutes away" alerts and a 2-crore-user Diwali campaign — through push, SMS and email. The campaign must never delay an OTP, and nobody wants the same SMS twice.

## In one line
**Producers drop notification requests into priority queues; workers apply user preferences and templates, then send through channel-specific providers with retries, dedupe and fallbacks.**

☕ **Darshini:** the order announcer: urgent parcel orders are called out on a separate mic from the general "today's special" announcements.

## R · Requirements
- Channels: push, SMS, email, in-app · triggered by services and campaigns
- Preferences, opt-outs, quiet hours, templates in Kannada/English/Hindi
- OTP fast + reliable; campaigns high-throughput; no duplicates

## E · Estimation
Transactional ~50/s (peak 500/s) · campaign 2 crore in 30 min ≈ **11,000/s**

## A · API
```
POST /v1/notifications { user_id, type, data, priority, channels, idempotency_key } → 202
POST /v1/campaigns { segment_id, template_id, schedule_at }
```

## D · Data
templates · user_preferences · devices (push tokens) · notification log (TTL) · inbox

## H · High-level design
```
Producers → Notification API (validate, dedupe)
   → Queues by PRIORITY: high (OTP, orders) | low (promos)
   → Workers: prefs + template + quiet hours
   → Channel queues: push | sms | email | in-app → providers (APNs/FCM, SMS, email)
   ← delivery callbacks → status/logs
```

## D · Deep dives
- **Separate lanes** (bulkhead): campaigns never block OTPs
- **At-least-once + dedupe** with idempotency key · retries with backoff · DLQ
- **Fallback:** critical push not delivered → SMS · multiple SMS providers
- Provider rate limits · frequency caps · collapse keys

## 🎯 Interview Line
**Q: How do you ensure OTPs aren't delayed by a huge marketing campaign?**
> Separate the pipelines: high-priority transactional notifications get their own queues, workers and reserved provider capacity, while campaigns go through low-priority queues that are throttled. That bulkhead means a 2-crore-message campaign can never sit in front of an OTP.

## Key words
`Priority queue` · `Bulkhead` · `Idempotency key` · `Dedupe` · `Fallback channel` · `DLQ` · `Frequency cap`
