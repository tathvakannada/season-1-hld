# 03 · Networking you actually need (IP, ports, TCP vs UDP, HTTPS)

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [02](../02-client-server-dns-http/README.md) · ⏭️ [04 · APIs explained](../04-apis-rest/README.md)

## In one line
**IP finds the machine, the port finds the program, TCP delivers reliably (slower), UDP delivers fast (no guarantee), and HTTPS is HTTP inside an encrypted tunnel.**

## The 4 layers (enough for system design)
| Layer | Examples | Job |
|---|---|---|
| Application | HTTP, HTTPS, DNS | What your code speaks |
| Transport | **TCP, UDP** | How data is delivered between programs |
| Internet | **IP** | How data finds the machine |
| Link | Wi-Fi, 4G, cable | The physical road |

Like a **letter → envelope → courier bag → road**. (College teaches 7 OSI layers; these 4 are enough here.)

## IP address and ports
- **IP address** = address of the machine (the building).
- **Port** = which program on that machine (the counter number inside the building).
- Full address = `IP:port`, e.g. `13.235.10.20:443`.

| Port | Used by |
|---|---|
| 80 | HTTP |
| 443 | HTTPS |
| 5432 | PostgreSQL |
| 6379 | Redis |

### Public vs private IP
- **Public IP**: reachable from the internet.
- **Private IP**: only inside a network (`10.x.x.x`, `172.16–31.x.x`, `192.168.x.x`).
- In real systems, only the **load balancer** is public. App servers and the **database stay in a private network** — the database is never exposed to the internet.

## TCP — reliable delivery
**3-way handshake** before any data:
1. **SYN** – "Hello, can we talk?"
2. **SYN-ACK** – "Yes! Can you hear me?"
3. **ACK** – "Yes, let's go."

➡️ Costs **one full round trip before sending data**.

TCP guarantees: ✅ delivery (re-sends lost packets) ✅ order ✅ no duplicates.
Cost: ❌ slower; ❌ one lost packet makes the others wait (*head-of-line blocking*).
**Used for:** websites, APIs, payments, database connections.

## UDP — fast, no promises
No handshake, no re-sending, no ordering. Just send.
**Used for:** video/voice calls, online games, live streaming, DNS, and **QUIC (HTTP/3)**.

## TCP vs UDP
| | TCP | UDP |
|---|---|---|
| Handshake | Yes | No |
| Delivery guaranteed | Yes | No |
| Order guaranteed | Yes | No |
| Speed | Slower | Faster |
| Examples | Web, APIs, DB | Video calls, games, DNS |
| ☕ Darshini | Registered post with acknowledgement | Shouting "Masala dose ready!" in the hall |

Back to Oota: **order confirmation → TCP**. **Rider live location** → losing one update is fine, so UDP-style delivery is acceptable (many apps still use TCP-based WebSockets for simplicity).

## HTTP versions (bonus)
- **HTTP/1.1** – one request at a time per connection.
- **HTTP/2** – many requests at once over one TCP connection.
- **HTTP/3** – runs on **QUIC**, which is built on **UDP** (QUIC adds its own reliability).

## HTTPS = HTTP + TLS 🔒
TLS gives:
1. **Encryption** – nobody in between can read it.
2. **Integrity** – nobody can change it without being detected.
3. **Authentication** – a **certificate** proves you're talking to the real `oota.com`.

⚠️ HTTPS secures the *connection*, not the server's code.

## Distance = latency
- Light in fibre ≈ **5 ms per 1,000 km** (one way); real routes are longer.
- Same city/data centre: a few ms. India ↔ USA: around **200+ ms** round trip.
- A new HTTPS connection needs **several round trips** (DNS + TCP + TLS + request).
- We can't make light faster, so we move servers **closer to users** → **CDN** (Phase 1).

## 🎯 Interview Line
**Q: TCP vs UDP — when would you use each?**
> TCP when correctness and order matter — APIs, payments, database connections, file transfer. UDP when low latency matters more than completeness and the app can tolerate loss — voice/video calls, gaming, DNS, and as the base of QUIC/HTTP3.

## Key words
`IP address` · `Port` · `Public/Private IP` · `TCP` · `3-way handshake` · `UDP` · `Head-of-line blocking` · `QUIC` · `HTTPS` · `TLS` · `Certificate` · `Latency`
