# 46 · The System Design Interview framework

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [45](../../phase-5-interview-mode/45-back-of-the-envelope-estimation/README.md) · ⏭️ [47 · Design a URL shortener (bit.ly)](../../phase-6-case-studies/47-design-url-shortener/README.md)

## The problem
Candidate hears "Design WhatsApp", instantly draws 30 boxes with Kafka and Cassandra, gets lost in one detail, and runs out of time — never discussed requirements or trade-offs.

## In one line
**A system design interview is a 45-minute guided conversation: clarify, size, define the interface, sketch a working design, then go deep and discuss trade-offs. A fixed structure keeps you calm and in control.**

☕ **Darshini:** ordering at a darshini: first decide **what** you want (requirements), **how many** people (estimation), place the order (API), then the cook plans tawas and helpers (design).

## R-E-A-D-H-D — the 6 steps
| # | Step | Time | Output on the board |
|---|---|---|---|
| R | **Requirements** | ~5 min | functional (3–5 features) + non-functional (scale, latency, availability, consistency) + out of scope |
| E | **Estimation** | ~3–5 min | QPS (avg/peak), storage, bandwidth → "so this means…" |
| A | **API** | ~3–5 min | key endpoints/events |
| D | **Data model** | ~3–5 min | entities, keys, which store and why |
| H | **High-level design** | ~10 min | boxes + arrows; walk one request end to end |
| D | **Deep dives** | ~10–15 min | bottlenecks, failures, trade-offs, wrap-up |
> "READ the problem, then go HD."

## What interviewers score
Clarifying · structure · technical depth · trade-offs · communication

## Habits that score
- Think aloud; check in ("should I go deeper on X?")
- Simple and **working** first, then smart
- Every box has a reason; every choice has a trade-off
- Accept hints — they save you time
- End with a 30-second summary + "with more time I'd…"

## Common mistakes
Jumping to boxes · no numbers · over-engineering · ignoring failures · going silent · 20 minutes on one box

## Every case study in this season follows R-E-A-D-H-D
URL shortener → rate limiter → notifications → chat → feed → YouTube → Uber → Oota → BookMyShow → Drive → autocomplete → crawler → payments → KV store → leaderboard → scheduler

## 🎯 Interview Line
**Q: How do you approach a system design interview?**
> I clarify functional and non-functional requirements and scope, do a quick estimate to see what's actually hard, define the APIs and data model, sketch a simple working high-level design and walk a request through it, then spend the remaining time on deep dives — bottlenecks, failures and trade-offs — and close with a summary.

## Key words
`Functional requirements` · `Non-functional requirements` · `Scope` · `High-level design` · `Deep dive` · `Trade-off`
