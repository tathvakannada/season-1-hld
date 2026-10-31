# 61 · Design a payment system (UPI-style)

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [60](../../phase-6-case-studies/60-design-web-crawler/README.md) · ⏭️ [62 · Design a distributed key-value store](../../phase-6-case-studies/62-design-key-value-store/README.md)

## The problem
You tap Pay ₹500 on Oota, the network drops, you tap again. The bank says "pending". Did money move? Twice? Every rupee must be accounted for.

## In one line
**Make every step idempotent, never assume on a timeout, record money in a double-entry ledger, and reconcile with the bank every day.**

☕ **Darshini:** the cashier's ledger book: every rupee is written twice (cash drawer ↑, dosa sales ↑), never erased, and tallied with the bank every night.

## R · Requirements
Pay-in · refunds · payouts · status · **no double charge, no lost money, full audit**, secure (PCI-DSS)

## E · Estimation
~115 payments/s avg, peak ~500/s — the hard part is **correctness**, not scale

## A · API
`POST /v1/payments` (Idempotency-Key = order_id) · `GET /v1/payments/{id}` · `POST /webhooks/psp` · refunds

## D · Data
payments (state machine, order_id UNIQUE) · payment_events (audit) · **ledger_entries (double-entry, immutable)** · amounts in **paise** (integers)

## H · High-level design
```
Order svc → Payment svc (idempotency) → Executor → PSP / bank / UPI
          ← response (often PENDING) + webhook (final)
          → state machine → Ledger → events (outbox)
Reconciliation job: our ledger ⇄ bank settlement files
```

## D · Deep dives
- **Idempotency at every hop:** client → us, us → PSP, webhooks (duplicates/out of order)
- **Timeout = UNKNOWN**, not failed → poll status + wait for webhook
- **Double-entry:** every transaction's debits = credits → errors detectable
- **Reconciliation** daily: matched · missing on our side · missing on theirs · amount mismatch
- Tokenise cards (never store raw), verify webhook signatures, fraud checks

## 🎯 Interview Line
**Q: How do you guarantee a customer is never charged twice?**
> Idempotency at every hop: the client sends an idempotency key like the order ID, which is unique in our payments table, so retries return the original result; we pass our payment ID to the provider as its idempotency reference; and webhooks are applied through a state machine that ignores duplicates. Reconciliation with bank settlement files catches anything that slips through.

## Key words
`Idempotency` · `PSP` · `Webhook` · `Double-entry ledger` · `Reconciliation` · `Paise (integers)` · `Tokenisation`
