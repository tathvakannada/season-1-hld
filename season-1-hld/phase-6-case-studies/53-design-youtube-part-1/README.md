# 53 · Design YouTube — Part 1: upload, transcoding, playback

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [52](../../phase-6-case-studies/52-design-news-feed/README.md) · ⏭️ [54 · Design YouTube — Part 2: live streaming at IPL scale](../../phase-6-case-studies/54-design-live-streaming/README.md)

## The problem
A creator uploads a 2 GB, 4K video. Viewers on Jio 4G in a village and on fibre in Bengaluru must both watch it smoothly, starting in under 2 seconds.

## In one line
**Upload directly to object storage, split the video into chunks, transcode them in parallel into many qualities, package into small segments with a manifest, serve from CDNs, and let the player switch quality per segment.**

☕ **Darshini:** one big batch of dosa batter is spread across many tawas at once (parallel transcoding) and served in small plates (segments); each customer chooses plate size by appetite (bitrate).

## R · Requirements
Upload big files (resumable) · watch on any device/network · fast start, little buffering · cost-efficient storage

## E · Estimation
5 lakh uploads/day × 300 MB → ~150 TB/day raw → **petabytes/month** · egress in Tbps → **CDN serves almost everything**

## A · API
`POST /v1/videos/uploads` → presigned part URLs · `…/complete` · `GET /v1/videos/{id}` → manifest URL

## D · Data
videos (status: UPLOADING → PROCESSING → READY) · renditions · object storage: raw/, segments/, manifests

## H · High-level design
```
UPLOAD : App → presigned multipart → Object storage (raw) → event → queue
PROCESS: split → transcode chunks in PARALLEL → 240p…1080p ladder
         → HLS/DASH segments (2–6 s) + manifest → thumbnails → READY
PLAY   : Player → manifest → CDN edge → (miss) origin → storage
```

## D · Deep dives
- **Multipart upload:** retry failed parts only, resume later
- **Parallel transcoding:** chunks → many workers → minutes, not hours; idempotent tasks
- **ABR (adaptive bitrate):** player measures bandwidth → picks quality for the next segment → quality drops instead of buffering
- **Cost:** storage tiers, codecs (AV1/VP9 save bandwidth), delete unpopular renditions

## 🎯 Interview Line
**Q: How does YouTube adapt video quality to a bad network?**
> Each video is transcoded into a ladder of qualities, each cut into short segments listed in a manifest (HLS/DASH). The player measures throughput and buffer level and picks the best quality for each next segment, so on a weak connection it drops to 360p instead of buffering.

## Key words
`Multipart upload` · `Transcoding` · `Bitrate ladder` · `HLS / DASH` · `Segment` · `Manifest` · `ABR` · `CDN`
