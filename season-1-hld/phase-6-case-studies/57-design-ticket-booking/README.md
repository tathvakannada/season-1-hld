# 57 · Design ticket booking (BookMyShow / IRCTC Tatkal)

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [56](../../phase-6-case-studies/56-design-food-delivery/README.md) · ⏭️ [58 · Design file storage & sync (Google Drive)](../../phase-6-case-studies/58-design-google-drive/README.md)

## The problem
10:00 am, Tatkal opens. Lakhs of people click the same few hundred seats in the same second. Two people must never get seat 23.

## In one line
**Hold seats briefly with an atomic lock or conditional update, take payment, then confirm — and put a virtual waiting room in front so the spike can't crush the database.**

☕ **Darshini:** the last 5 Mysore pak boxes: the cashier gives tokens (waiting room), keeps a box aside for you for 5 minutes (hold), and hands it over only after you pay.

## R · Requirements
Seat map · select → hold → pay → confirm · cancel/refund · waitlist (IRCTC)
**No double booking ever** · survive known spikes · fair · stop bots

## E · Estimation
Inventory is tiny (~300 seats/show, ~1,000/train) but 10⁴–10⁵ RPS in the first minute → **contention, not data size**

## A · API
`GET /shows/{id}/seats` (cached) · `POST /shows/{id}/holds` → 409 if taken · `POST /bookings` (Idempotency-Key)

## D · Data
`seats(show_id, seat_id, status AVAILABLE|HELD|BOOKED, hold_id, hold_expires_at)` + unique constraint

## H · High-level design
```
Users → WAF/bot checks → Virtual waiting room (tokens)
      → Booking svc → Seat DB (locks/conditional update) + Redis holds (TTL)
      → Payment → confirm → tickets
Expiry worker → releases expired holds
```

## D · Deep dives
**Hold → Pay → Confirm**
- Option 1: `SELECT … FOR UPDATE` (pessimistic lock)
- Option 2: `UPDATE … SET status='HELD' WHERE status='AVAILABLE'` (check rows affected)
- Option 3: Redis `SET seat NX EX 600` (fast), DB still the source of truth
- Holds expire (TTL + sweeper) · payment after expiry → re-hold or auto-refund
- **Waiting room** + rate limiting + caching + shard by show/train

## 🎯 Interview Line
**Q: Two users click the same seat at the same millisecond. What happens?**
> Both requests try to move the seat from AVAILABLE to HELD with an atomic operation — a row lock or a conditional update that checks the current status. Only one succeeds; the other sees zero rows updated and gets a 409 'seat taken'. The held seat has a short expiry so it's released if payment doesn't complete.

## Key words
`Seat hold` · `Pessimistic lock` · `Optimistic / conditional update` · `TTL` · `Virtual waiting room` · `Contention` · `Idempotency`
