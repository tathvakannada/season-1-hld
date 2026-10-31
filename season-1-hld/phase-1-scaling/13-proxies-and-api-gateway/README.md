# 13 · Proxies, Reverse Proxy & API Gateway

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [12](../12-cdn/README.md) · ⏭️ [14 · SPOF, redundancy & failover](../14-spof-redundancy-failover/README.md)

## The problem
Oota has several backend services. The app shouldn't need 5 addresses, and every service is re-writing the same login checks, rate limits and logging. We need **one front door**.

## In one line
**A proxy is a middleman. A forward proxy acts for clients; a reverse proxy acts for servers. An API gateway is a smart reverse proxy with auth, rate limits and routing.**

## Forward proxy (client side)
Clients → **proxy** → internet. The **server doesn't see the client**.
Used for: office/college filtering, shared caching, hiding the client's IP.

## Reverse proxy (server side)
Client → **proxy** → servers. The **client doesn't see the servers**.
Used for: load balancing, TLS termination, caching, compression, hiding internal servers, routing `/images` vs `/api`.
Examples: Nginx, HAProxy, Envoy, Traefik.

```
Forward:  [You] → [Proxy] → Internet → Server     (server can't see you)
Reverse:  You → Internet → [Proxy] → [Servers]    (you can't see the servers)
```

## API Gateway
A reverse proxy specialised for APIs:
- **Routing** — `/users` → user service, `/orders` → order service
- **Authentication** — check the token once at the edge
- **Rate limiting** — per user / API key
- **Aggregation** — one client call → several services → one response
- **Logging, metrics, tracing** in one place
Examples: Kong, AWS API Gateway, Apigee.

## LB vs reverse proxy vs gateway
| | Load balancer | Reverse proxy | API gateway |
|---|---|---|---|
| Main job | Spread traffic over identical servers | Front door: routing, TLS, caching | Front door for APIs with policies |
| Auth / rate limits | Rarely | Possible | Core feature |

They **overlap** — Nginx can be all three. In interviews, say **what job** the box is doing.

☕ Darshini: forward proxy = you send a friend to buy food for you. Reverse proxy = the hotel reception (you never see the kitchen). API gateway = reception + guard + token checker + "2 free coffees per person" rule.

## ⚖️ Trade-off corner
The gateway is on **every** request → must be highly available and scalable; adds a small hop; don't put business logic in it.

## 🎯 Interview Line
**Q: Forward vs reverse proxy?**
> A forward proxy sits in front of clients and acts on their behalf, hiding or controlling them. A reverse proxy sits in front of servers, hiding them and handling load balancing, TLS termination and caching. An API gateway is a reverse proxy that also does auth, rate limiting and routing for microservices.

## Key words
`Forward proxy` · `Reverse proxy` · `API gateway` · `TLS termination` · `Rate limiting` · `BFF`
