# 32 · API design deep dive

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [31](../../phase-3-communication/31-real-time-communication/README.md) · ⏭️ [33 · Phase 3 recap: Oota's order flow end to end](../../phase-3-communication/33-phase-3-recap/README.md)

## The problem
Order history returns 4,000 orders at once; double-taps cause double charges; renaming a field crashes old apps; the home screen needs 6 calls; internal services make millions of JSON calls per second.

## In one line
**Good APIs page big lists, are safe to retry, evolve without breaking clients, and use the right style for the caller.**

☕ **Darshini:** a webhook is "we'll call your number when the parcel is ready" — but you still check at closing time in case they forgot (reconciliation).

## Pagination
| Offset `?limit=20&offset=40` | Cursor `?limit=20&after=<cursor>` |
|---|---|
| ✅ simple, jump to page N | ✅ fast at any depth, stable with new items |
| ❌ slow deep pages, duplicates when items are inserted | ❌ no jumping to page N |

> 💡 **Offset for pages, cursor for feeds.**

## Idempotency keys
`POST /payments` + `Idempotency-Key: <uuid per attempt>` → server stores key → result; retries with the same key get the same result, no double charge. Keys expire (e.g. 24 h).

## Versioning & evolution
- `/v1/…` URL versions (or headers)
- ✅ safe: add optional fields, add endpoints
- ❌ breaking: remove/rename fields, change types
- Clients ignore unknown fields · deprecate with a sunset date
> 💡 **Add, don't rename.**

## REST vs gRPC vs GraphQL
| | REST | gRPC | GraphQL |
|---|---|---|---|
| Format | JSON/HTTP | Protobuf over HTTP/2 | JSON query |
| Strength | simple, universal | fast, typed, streaming | client picks fields, 1 round trip |
| Weakness | over/under-fetching | not browser-friendly | complex server, caching |
| Oota | public & mobile APIs | internal service calls | mobile home screen |

## Webhooks
Provider POSTs to your URL when something happens. **Verify the signature**, respond fast (enqueue), handle **duplicates idempotently**, reconcile periodically.

## 🎯 Interview Line
**Q: How do you make a payment API safe to retry?**
> The client sends an idempotency key per payment attempt; the server stores the key with the result and returns the stored result for repeats instead of charging again. State transitions are idempotent, webhooks are deduplicated, and a periodic reconciliation catches anything missed.

## Key words
`Offset pagination` · `Cursor pagination` · `Idempotency key` · `Versioning` · `Backward compatibility` · `gRPC` · `GraphQL` · `Webhook`
