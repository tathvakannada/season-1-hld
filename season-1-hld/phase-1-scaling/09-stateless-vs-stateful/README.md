# 09 · Stateless vs Stateful servers

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [08](../08-load-balancing/README.md) · ⏭️ [10 · Caching part 1](../10-caching-part-1/README.md)

## The problem
"I logged in, added biryani to my cart, refreshed… cart empty and I'm logged out!"
Request 1 went to Server A (which stored the session **in its memory**). Request 2 went to Server B, which knows nothing.

## In one line
**A stateless server remembers nothing about you between requests — so any server can handle any request.**

☕ Darshini: if your order is only in cook #1's head, cook #2 can't help. Write it on the token or the common order board — then any cook can serve you.

## What is "state"?
Data kept between requests: login session, cart, uploaded files on local disk, in-memory counters.

## Fix 1: Sticky sessions (band-aid)
LB always sends a user to the same server (cookie or IP hash).
- ❌ Uneven load · ❌ server crash still loses sessions · ❌ hurts autoscaling

## Fix 2: Move state out → stateless servers ✅
- **Shared session store** (e.g. **Redis**): `session:abc123 → {user_id: 42}`. Any server looks it up.
- **Signed tokens** (e.g. **JWT**): the client carries a signed token; any server verifies the signature. Hard to revoke early → use short expiry + refresh tokens.
- Files → **object storage**, carts → DB/Redis, scheduled jobs → a scheduler/queue.

```
LB → [App A] [App B] [App C]   ← identical, remember nothing
         \       |       /
       Redis (sessions) · DB · Object storage
```

> 💡 Stateless doesn't mean "no state anywhere" — state moves to components built for it.
> 💡 "Treat servers like cattle, not pets."

## ⚖️ Trade-off corner
| Session store | JWT |
|---|---|
| ✅ Easy logout/revoke | ✅ No lookup per request |
| ❌ Extra lookup, store must be highly available | ❌ Hard to revoke before expiry |

⚠️ Standard JWTs are **signed, not encrypted** — anyone can read the content. Never put secrets inside.

## 🎯 Interview Line
**Q: How do you make a web tier stateless?**
> Move all user state out of the servers: sessions into a shared store like Redis or into signed tokens, files into object storage, and carts/user data into the database or cache. Then any instance can serve any request, so we can add or remove servers freely.

## Key words
`State` · `Stateless` · `Sticky session` · `Session store` · `Redis` · `JWT` · `Object storage`
