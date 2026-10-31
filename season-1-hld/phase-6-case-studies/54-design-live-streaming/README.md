# 54 · Design YouTube — Part 2: live streaming at IPL scale

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [53](../../phase-6-case-studies/53-design-youtube-part-1/README.md) · ⏭️ [55 · Design ride hailing (Uber / Rapido / Namma Yatri)](../../phase-6-case-studies/55-design-ride-hailing/README.md)

## The problem
IPL final, last over, 3 crore people watching live. Everyone wants the same newest 4-second segment at the same moment — and the toss spike arrives in seconds.

## In one line
**Encode the live feed into a bitrate ladder, package tiny segments in real time, and push them through multiple CDNs with request collapsing, pre-scaling and graceful degradation.**

☕ **Darshini:** the day's special from one big pot: the main kitchen sends one pot per branch, not one plate per customer — and before the lunch rush, extra pots are already ready.

## R · Requirements
Ingest live feed · multiple qualities/languages · crores concurrent · low buffering · a few seconds of delay · **no second chance**

## E · Estimation
3 crore viewers × 2 Mbps ≈ **60 Tbps** → multi-CDN + ISP caches · ~10⁷ segment requests/s at edges

## H · Architecture
```
Stadium → redundant ingest → live transcoder (ladder) → packager (segments + live manifest)
        → origin (shielded) → multi-CDN edges → players (ABR)
Side: scorecard fan-out, concurrency counter, ads, login/entitlement
```

## D · Deep dives
- **Same newest segment for everyone** → cache at edges + **request collapsing** (1 lakh requests → 1 upstream fetch)
- **Latency vs stability:** normal HLS 15–30 s · low-latency HLS/DASH 2–5 s (more fragile)
- **Spikes:** pre-scale before the toss · multi-CDN steering · cap max bitrate when tight · switch off non-essentials
- **Often the APIs break first** (login, entitlement) → cache, rate-limit, longer tokens
- Redundant encoders and ingest paths; game-day drills

| VOD (Part 1) | Live (Part 2) |
|---|---|
| segments made once | segments made every few seconds |
| long-tail misses | everyone wants the same segment |
| autoscale fine | pre-scale before events |

## 🎯 Interview Line
**Q: How do you stop the origin from melting when 3 crore viewers request the newest segment?**
> Serve segments from CDN edges with an origin shield in between and enable request collapsing, so thousands of simultaneous misses for the same segment become one upstream fetch. Combine that with multi-CDN, pre-scaling before the match, and graceful degradation like capping bitrate.

## Key words
`Live ingest` · `Packager` · `Request collapsing` · `Origin shield` · `Multi-CDN` · `Low-latency HLS` · `Pre-scaling` · `Graceful degradation`
