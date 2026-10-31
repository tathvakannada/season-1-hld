# 08 · Load Balancing

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [07](../07-vertical-vs-horizontal-scaling/README.md) · ⏭️ [09 · Stateless vs Stateful](../09-stateless-vs-stateful/README.md)

## The problem
Oota has 4 servers, but `oota.com` points to one address. Who decides which server gets which request — and skips the dead ones?

## In one line
**A load balancer receives every request and forwards it to a healthy server, spreading the load.**

☕ Darshini: the **token counter** that sends each customer to a free cook.

## What it does
1. **One entry point** — users only know the LB's address.
2. **Distributes** requests using an algorithm.
3. **Health checks** — pings servers (e.g. `GET /health`); stops sending traffic to failed ones.
4. Enables **zero-downtime deploys** (take one server out, update, put back).
5. Often does **TLS termination** (decrypts HTTPS once).

```
             ┌─► App 1
Users ─► LB ─┼─► App 2
             ├─► App 3  ✗ failed health check → skipped
             └─► App 4
```

## Algorithms
| Algorithm | How | Good when |
|---|---|---|
| Round robin | 1, 2, 3, 4, 1, 2… | Equal servers, similar requests |
| Weighted round robin | Bigger servers get more turns | Different server sizes |
| Least connections | Server with fewest active connections | Long/uneven requests, WebSockets |
| IP / consistent hash | Same client → same server | Stickiness, cache locality |

## L4 vs L7
- **L4** — looks only at IP + port. Fast, simple. *Reads the envelope.*
- **L7** — reads the HTTP request (URL, headers). Can route `/api` and `/images` differently. *Reads the letter.*

## Don't let the LB be a single point of failure
Run **two**: active–passive (standby takes over the IP) or active–active. Managed cloud LBs are redundant by design.

**Real world:** Nginx, HAProxy, Envoy, AWS ALB/NLB.

## ⚖️ Trade-off corner
- A load balancer improves **throughput and availability**, not the speed of a single request (it adds a tiny hop).
- Smarter algorithms need more state and work.
- Health checks that are too strict can remove *all* servers during a small DB blip.

## 🎯 Interview Line
**Q: How does a load balancer know a server is down?**
> Through health checks — it calls a health endpoint at intervals and removes a server after several failures; it may also detect failing real requests. The server is added back once it passes checks again. To avoid the LB itself being a single point of failure, we run redundant LBs.

## Key words
`Load balancer` · `Health check` · `Round robin` · `Least connections` · `L4/L7` · `TLS termination` · `Active-passive`
