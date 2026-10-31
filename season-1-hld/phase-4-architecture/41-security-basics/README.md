# 41 · Security basics for system design

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [40](../../phase-4-architecture/40-observability/README.md) · ⏭️ [42 · Special data structures: Bloom filter, HyperLogLog, Geohash, Trie](../../phase-4-architecture/42-special-data-structures/README.md)

## The problem
A rider sees other riders' earnings by changing an ID in the URL; a leaked repo contains the DB password; bots flood login; old code builds SQL from user input.

## In one line
**Know who's calling (authN), check what they may do (authZ), encrypt in transit and at rest, keep secrets out of code, and put a defence in front of each common attack.**

☕ **Darshini:** the **guard** checks your ID (authN); your **token colour** decides if you can enter the kitchen (authZ); the **locker** for cash (encryption at rest); the **cash van** (encryption in transit).

## Authentication — who are you?
- Passwords: slow salted **hashes** (bcrypt, scrypt, Argon2) — never plain text or reversible encryption; MFA/OTP
- Sessions or short-lived **JWT** + refresh tokens
- **OAuth 2.0** (authorization code + PKCE) + **OIDC** = "Login with Google"
- Service-to-service: **mTLS**

## Authorization — what can you do?
- **RBAC** (customer, restaurant owner, rider, admin) or policy-based rules
- **Check ownership on every request** — never trust IDs from the client (IDOR / broken object-level auth)
- Least privilege everywhere

## Encryption & secrets
- TLS in transit (internal too) · encryption at rest (KMS)
- Don't store cards — use the payment gateway's tokens
- Secrets in a **secrets manager** (Vault, AWS Secrets Manager), rotated, never in code/logs

## Attack → defence
| Attack | Defence |
|---|---|
| SQL injection | parameterised queries |
| XSS | output encoding, CSP |
| CSRF | SameSite cookies, CSRF tokens |
| SSRF | outbound allow-lists |
| IDOR | ownership checks |
| brute force | rate limiting, MFA |
| DDoS | CDN/WAF, rate limiting, autoscaling |

## 🎯 Interview Line
**Q: What's the difference between authentication and authorization, and how does 'Login with Google' work?**
> Authentication verifies who you are; authorization decides what you're allowed to do. 'Login with Google' uses OAuth 2.0's authorization code flow with PKCE plus OpenID Connect: the app redirects to Google, the user signs in and consents, the app exchanges a short code for tokens, and the ID token tells it who the user is.

## Key words
`Authentication` · `Authorization` · `Hashing` · `JWT` · `OAuth 2.0` · `OIDC` · `PKCE` · `mTLS` · `RBAC` · `IDOR` · `OWASP`
