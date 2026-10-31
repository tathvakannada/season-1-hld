# 04 · APIs explained (REST, methods, status codes, JSON)

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [03](../03-networking-essentials/README.md) · ⏭️ [05 · Databases 101](../05-databases-101/README.md)

## In one line
**An API is a contract: "send me a request in this format, I'll reply in that format."** Any client — app, website, another service — can use the backend without knowing how it works inside.

## Why not let apps talk to the database directly?
- Every app would contain the **DB password** → security disaster.
- Change one table → **every app breaks**.
- Business rules (discounts, stock checks) get **copied** into every app.
✅ So: apps → **API** → backend → database. Only the backend touches the DB.

## ☕ Darshini analogy
The **menu card + the fixed way of ordering** ("one masala dose, parcel") = the **API**.
How the kitchen cooks (gas or induction, which cook) = the **implementation**. It can change; the menu stays the same.

## REST in 3 ideas
1. **Everything is a resource (noun) with a URL**: `/restaurants`, `/restaurants/42`, `/restaurants/42/menu`, `/orders/1023`
2. **HTTP methods are the verbs:**

| Method | Meaning | Example | Safe? | Idempotent? |
|---|---|---|---|---|
| GET | Read | `GET /restaurants/42` | ✅ | ✅ |
| POST | Create / action | `POST /orders` | ❌ | ❌ |
| PUT | Replace fully | `PUT /users/7/address` | ❌ | ✅ |
| PATCH | Update partly | `PATCH /orders/1023` | ❌ | not always |
| DELETE | Remove | `DELETE /cart/items/5` | ❌ | ✅ |

- **Safe** = doesn't change data. **Idempotent** = doing it once or 10 times gives the same final result.

3. **Stateless**: every request carries everything needed (e.g. the login token).

## Status codes (learn by family)
- **2xx** ✅ success — 200 OK, 201 Created
- **3xx** ↪️ go elsewhere — 301, 302, 304
- **4xx** 🙋 *you* made a mistake — 400, 401, 403, 404, 429
- **5xx** 🔥 *we* made a mistake — 500, 502, 503, 504

> **401** = "Who are you?" (not logged in) · **403** = "I know you. Still no."

## Example
```http
POST /v1/orders
Idempotency-Key: 7f3a-91c2

{ "restaurant_id": 42, "items": [{"item_id": 7, "qty": 2}], "address_id": 3 }
```
```http
201 Created

{ "order_id": 1023, "status": "PLACED", "total": 240 }
```

## Idempotency 🛗
Pressing a lift button 10 times calls **one** lift — that's idempotent.
Tapping "Pay ₹240" twice must not charge twice. Fix: the app sends an **Idempotency-Key** (a unique ID per payment attempt). Same key again → the server returns the first result instead of charging again.

## Oota's first APIs
```
GET    /v1/restaurants?city=bengaluru     list restaurants
GET    /v1/restaurants/{id}/menu          menu
POST   /v1/cart/items                     add to cart
POST   /v1/orders  (+ Idempotency-Key)    place order
GET    /v1/orders/{id}                    order status
```

## Good habits
- Nouns, not verbs: `POST /orders` ✅ — `POST /createOrder` ❌
- Filters in query params: `/restaurants?city=bengaluru`
- Use real status codes — never `200 OK` with an error inside
- Version your API: `/v1/...`

## ⚖️ Trade-off corner
REST is simple and works everywhere, but not best for everything: **gRPC** (fast service-to-service), **GraphQL** (client picks fields), **WebSockets** (live updates). Coming later.

## 🎯 Interview Line
**Q: PUT vs PATCH vs POST?**
> PUT replaces the whole resource and is idempotent. PATCH updates only some fields and isn't necessarily idempotent. POST creates a resource or triggers an action and isn't idempotent — so for payments we add an idempotency key.

## Key words
`API` · `REST` · `Resource` · `HTTP method` · `Status code` · `JSON` · `Safe` · `Idempotent` · `Idempotency key`
