# 02 · What happens when you open an app? (Client–Server, DNS, HTTP)

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [01](../01-what-is-system-design/README.md) · ⏭️ [03 · Networking essentials](../03-networking-essentials/README.md)

## In one line
**Your phone (client) finds the server's address using DNS, sends it a request, and the server sends back a response.** Almost everything in system design is a version of this loop.

## ☕ Darshini analogy
| Darshini | Tech |
|---|---|
| Customer | **Client** |
| Looking up the hotel's address | **DNS** |
| The hotel kitchen | **Server** |
| Your order | **Request** |
| The plate of food | **Response** |

## Client and Server
- **Client** = the one who **asks**: browser, mobile app, even another server.
- **Server** = the one who **answers**. It is **just a normal computer** running a program that waits for requests 24×7.
- These are **roles**: Oota's backend is a *server* for your phone but a *client* when it calls the payment gateway.

## Domain names and IP addresses
- Computers find each other using **IP addresses** (e.g. `13.235.10.20`).
- Humans use **domain names** (e.g. `oota.com`).
- **DNS** converts names to IPs. **DNS = the phonebook of the internet.**

## How DNS finds the address
1. Browser cache → 2. OS cache → 3. **DNS resolver** (your ISP's or public, like 8.8.8.8)
4. Resolver asks a **root server**: "who handles `.com`?"
5. Asks the **`.com` TLD server**: "who handles `oota.com`?"
6. Asks Oota's **authoritative server**: "what's the IP?" → `13.235.10.20`

The answer is **cached** along the way for a time called **TTL (Time To Live)**. So the full chain happens rarely.

### TTL trade-off
| Long TTL | Short TTL |
|---|---|
| ✅ Fewer lookups, faster | ✅ Changes spread quickly |
| ❌ If the server IP changes, users keep going to the old one until TTL expires | ❌ Many more lookups |

💡 Tip: companies **lower the TTL before moving servers**, so the switch is quick.

## HTTP request (like a letter)
```
GET /restaurants?city=bengaluru HTTP/1.1     ← method + path + query
Host: oota.com                               ← headers (envelope info)
Authorization: Bearer abc123
Accept: application/json
                                             ← body (empty for GET)
```
- **Method** – what you want (GET = give me data, POST = create something)
- **Path** – which resource (`/restaurants`)
- **Query** – filters (`city=bengaluru`)
- **Headers** – extra info (who you are, format you accept)
- **Body** – the data you send (for POST/PUT)

## HTTP response
```
HTTP/1.1 200 OK                              ← status code
Content-Type: application/json               ← headers

[{"id": 42, "name": "Namma Tiffins", "rating": 4.7}]   ← body
```
Common status codes: **200** OK · **404** Not Found · **500** Server Error.

## The full journey (type a URL → see the page)
1. You type the URL.
2. **DNS** turns the name into an IP.
3. **TCP handshake** – connect to the server.
4. **TLS handshake** – set up encryption (HTTPS).
5. Browser sends the **HTTP request**.
6. Server does the work (logic, database).
7. Server sends the **HTTP response**.
8. Browser **renders** the page and makes more requests for images/CSS/JS (often from a CDN).

## Oota so far
- Running on a laptop at home → if it sleeps, Oota is down; if the home IP changes, DNS breaks.
- Fix: move to a **cloud server (VM)** with a fixed IP. Still just **one** server — Phase 1 fixes that.

## ⚖️ Trade-off corner
Caching (DNS, browser) makes things **fast**, but cached data can be **old**. Speed vs freshness is a trade-off we'll see in every phase.

## 🎯 Interview Line
**Q: What happens when you type a URL in the browser?**
> The browser checks its caches, then DNS resolves the domain to an IP. It opens a TCP connection (plus a TLS handshake for HTTPS) and sends an HTTP request. The request reaches the server — usually through a load balancer — and the app builds a reply using caches/databases. The server returns an HTTP response with a status code and body, and the browser renders it, fetching images/CSS/JS, often from a CDN.

## Key words
`Client` · `Server` · `IP address` · `Domain name` · `DNS` · `Resolver` · `TTL` · `HTTP request` · `HTTP response` · `Status code`
