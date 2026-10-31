# 56 · Design food delivery — the Oota finale

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [55](../../phase-6-case-studies/55-design-ride-hailing/README.md) · ⏭️ [57 · Design ticket booking (BookMyShow / IRCTC Tatkal)](../../phase-6-case-studies/57-design-ticket-booking/README.md)

## The problem
8 pm on a match night. Lakhs of people browse Oota, place orders, pay, restaurants cook, delivery partners ride — and every order must be correct even when phones lose network halfway.

## In one line
**Cache everything people browse, protect checkout with idempotency and a saga across order, payment and restaurant, model each order as a state machine of events, and dispatch partners in small batches.**

☕ **Darshini:** the whole darshini at full rush: a menu board everyone reads (cache), a cashier who never bills twice (idempotency), a token that moves from counter to kitchen to pickup (state machine).

## R · Requirements
Discover restaurants · menu · cart · order + pay · restaurant accepts · partner assigned · live tracking · notifications
Browse: fast, slightly stale OK · Checkout: **correct, never double-charge** · peak spikes

## E · Estimation
1 crore orders/day → peak ~350–500 orders/s · browsing ~10⁴–10⁵ RPS · partner GPS ~10⁵ writes/s

## A · API
`GET /restaurants?lat&lng` · `GET /restaurants/{id}/menu` · `POST /orders` (**Idempotency-Key**) · live updates via WS/SSE

## D · Data
restaurants + search index (geo + text) · menu (cached) · orders (SQL) · order_events · partner geo index

## H · Season 1 on one canvas
```
App → CDN → Gateway (auth, rate limit)
  Search svc → Elasticsearch + Redis
  Order svc (SQL) ⇄ Payment svc (saga) → Kafka events
     → Restaurant svc · Dispatch svc ← Partner location · Notification svc · Tracking (WS)
```

## D · Deep dives
- **Order state machine:** PENDING_PAYMENT → PLACED → ACCEPTED → READY → PICKED_UP → DELIVERED (or CANCELLED → REFUND)
- **Saga + outbox:** payment OK but restaurant rejects → compensate with refund
- **Dispatch:** match partner arrival to food-ready time, batch nearby orders, assign atomically
- **Peak:** cached listings, degrade personalisation, protect checkout above all

## 🎯 Interview Line
**Q: How do you keep an order and its payment consistent across microservices?**
> Use a saga: the order starts as pending payment, the payment service charges with an idempotency key, and events move the order forward; if a later step fails — say the restaurant rejects — a compensating refund runs. The outbox pattern makes each state change and its event atomic, and a reconciliation job catches anything left in between.

## Key words
`Saga` · `Outbox` · `Order state machine` · `Idempotency key` · `Dispatch` · `Graceful degradation` · `Event-driven`
