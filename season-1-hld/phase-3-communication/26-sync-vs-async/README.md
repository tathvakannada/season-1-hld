# 26 · Sync vs Async communication

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [25](../../phase-2-data/25-choosing-a-database/README.md) · ⏭️ [27 · Message Queues](../../phase-3-communication/27-message-queues/README.md)

## The problem
"Place order" does payment, save, restaurant, rider, SMS, email, analytics, loyalty — all in one request. **12 seconds.** SMS provider down → the whole order fails.

## In one line
**Do only what the user needs right now synchronously; push everything else to the background.**

☕ **Darshini:** **sync** = cashier waits for your dosa before serving the next customer (queue explodes). **async** = cashier gives you a **token** and serves the next person; the kitchen calls your number.

## Synchronous (request–response)
Caller **waits** for the answer.
- ✅ simple, immediate result, easy error handling
- ❌ latency adds up, **failures cascade**, tight coupling

## Asynchronous (messages / events)
Caller hands off work and returns; **workers** do it later.
- ✅ fast response, absorbs spikes, failures isolated, decoupled
- ❌ result not immediate, harder debugging, duplicates/ordering, eventual consistency

## What stays sync?
Ask: **"Does the user need this result right now?"**
| Step | Sync / Async |
|---|---|
| validate cart & price | sync |
| authorise payment | sync |
| save order | sync |
| notify restaurant, assign rider | async |
| SMS / email / push | async |
| analytics, loyalty points | async |

Result: "Order placed ✅" in ~300 ms; progress shown with live updates.

> ⚠️ Every sync call needs a **timeout**.

## 🎯 Interview Line
**Q: When would you use synchronous vs asynchronous communication?**
> Synchronous when the caller needs the result to continue — like payment authorisation or validation. Asynchronous for slow, non-critical or retryable work — notifications, analytics, heavy processing — so the user gets a fast response and failures don't cascade.

## Key words
`Synchronous` · `Asynchronous` · `Blocking` · `Cascading failure` · `Decoupling` · `Timeout`
