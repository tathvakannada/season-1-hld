# 55 · Design ride hailing (Uber / Rapido / Namma Yatri)

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [54](../../phase-6-case-studies/54-design-live-streaming/README.md) · ⏭️ [56 · Design food delivery — the Oota finale](../../phase-6-case-studies/56-design-food-delivery/README.md)

## The problem
It's raining in Bengaluru at 6 pm. 50 lakh drivers send their location every 4 seconds, lakhs of riders tap "Book", and one driver must never be given two rides.

## In one line
**Keep only each driver's latest location in an in-memory geo index split into cells, search the rider's cell and its neighbours, offer the ride driver by driver, and assign with an atomic compare-and-set.**

☕ **Darshini:** the auto stand: the coordinator looks only at autos in his area and the next lane, calls one driver; if he says no, the next one — and an auto already on a trip is never called.

## R · Requirements
Drivers stream location · rider sees nearby cars + estimate · match · live tracking · fare
**No double assignment** (strong) · location updates eventually consistent

## E · Estimation
50 lakh drivers ÷ 4 s ≈ **12.5 lakh location writes/s** · latest location only ≈ 500 MB in memory

## A · API
`POST /drivers/me/location` (socket) · `GET /rides/estimate` · `POST /rides` · `POST /rides/{id}/accept` · live WS

## D · Data
In-memory **geo index**: cell (geohash/H3) → driver IDs · drivers(status) · rides (SQL, state machine) · history → Kafka

## H · High-level design
```
Driver → Location svc → Geo index (in memory, by city) + Kafka (history)
Rider → Ride svc → Pricing (surge) → Matching: nearby AVAILABLE drivers
      → rank by ETA → offer → accept → atomic assign
Trip: location → pub/sub → rider's socket
```

## D · Deep dives
- **Nearby search:** pickup cell + neighbours → candidates → ETA
- **No SQL write per ping** — latest in memory, history in Kafka
- **Assignment:** `UPDATE drivers SET status='ON_TRIP' WHERE id=? AND status='AVAILABLE'` → 0 rows = taken → next driver
- **Surge:** demand vs supply per cell from streaming

## 🎯 Interview Line
**Q: How do you make sure one driver isn't matched to two riders at once?**
> Assignment is an atomic conditional update on the driver's status — set ON_TRIP only where the status is still AVAILABLE, or an equivalent compare-and-set/lock in Redis. If it affects zero rows another ride got the driver first, so matching moves on to the next candidate.

## Key words
`Geohash / H3` · `Geo index` · `Matching` · `Compare-and-set` · `Surge pricing` · `Ride state machine` · `ETA`
