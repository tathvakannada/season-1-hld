# 64 · Design a distributed job scheduler

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [63](../../phase-6-case-studies/63-design-leaderboard/README.md) · ⏭️ [65 · Season 1 finale — the whole map](../../phase-6-case-studies/65-season-1-finale/README.md)

## The problem
"Send this reminder at 7 pm", "pay restaurants at 2 am", "retry in 5 minutes" — crores of jobs. One cron server would be a single point of failure, and a job must not run twice.

## In one line
**Store jobs by next run time, split them into buckets owned by schedulers through leases, push due jobs to a queue for workers, retry with backoff, and make every job idempotent.**

☕ **Darshini:** order tickets pinned by time on a rail: any free cook takes the next ticket; if a cook walks away mid-order, the ticket goes back on the rail.

## R · Requirements
One-time + recurring (cron) jobs · execute · retries, timeouts, history · cancel/pause
**Every job runs** (effectively once) · on time (seconds) · no single point of failure

## E · Estimation
10 crore jobs/day ≈ 1,150/s avg, but **50–100× spikes at round times** (00:00, 09:00)

## A · API
`POST /v1/jobs {schedule | run_at, target, payload, retry_policy}` · `GET /jobs/{id}/runs` · pause/cancel

## D · Data
jobs(next_run_at, partitioned into buckets) · job_runs **UNIQUE(job_id, scheduled_at)**

## H · High-level design
```
API → Jobs DB (buckets)
Schedulers (each owns buckets via LEASE): find due → create run → enqueue → next_run_at
Queue → Workers (autoscaled) → execute → ack
   fail → retry with backoff → DLQ + alert
```

## D · Deep dives
- **Find due jobs:** index on next_run_at, or time buckets / timing wheel / Redis sorted set
- **No double scheduling:** bucket leases (etcd/ZooKeeper) + unique run row
- **Exactly-once is a myth** → at-least-once + idempotent jobs (run_id as key)
- **Midnight herd:** jitter, prefetch, queue buffer, priorities
- **Long jobs:** heartbeats; missing heartbeat → retry elsewhere

## 🎯 Interview Line
**Q: How do you make sure a scheduled job doesn't run twice?**
> You can't fully guarantee it, so you design for at-least-once and make it harmless: each occurrence gets a unique run row keyed by job ID and scheduled time, buckets of jobs are owned by one scheduler at a time through leases, and the job handler is idempotent using the run ID as an idempotency key.

## Key words
`Cron` · `Lease` · `Bucket` · `Timing wheel` · `At-least-once` · `Idempotent job` · `Dead-letter queue` · `Heartbeat`
