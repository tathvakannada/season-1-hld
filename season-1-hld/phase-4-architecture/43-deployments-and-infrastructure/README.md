# 43 · Deployments & infra: containers, Kubernetes, CI/CD, canary

> 🎥 Video: *(add YouTube link)* · 📝 [Homework](HOMEWORK.md) · ⏮️ [42](../../phase-4-architecture/42-special-data-structures/README.md) · ⏭️ [44 · Phase 4 recap: Oota as a real company](../../phase-4-architecture/44-phase-4-recap/README.md)

## The problem
"Works on my machine", midnight manual deploys, a bad release hit 100% of users and broke checkout for 40 minutes, and someone scales servers by hand at lunch.

## In one line
**Package services in containers, run them on Kubernetes, ship through CI/CD, and release safely with rolling, blue-green or canary deployments plus feature flags.**

☕ **Darshini:** container = **tiffin box** (same meal everywhere) · Kubernetes = the **kitchen manager** keeping 10 cooks working · canary = the **food taster** · blue-green = **two identical kitchens**, switch the counter.

## Containers (Docker)
Image = app + runtime + libraries → runs the same everywhere; starts in seconds.

## Kubernetes in one picture
You declare **desired state**; Kubernetes keeps reality matching it.
- **Pod** (containers) · **Deployment** (replicas, rolling updates) · **Service** (stable name + load balancing) · **Ingress** · **ConfigMap/Secret** · **HPA** (autoscaling)
- Self-healing; readiness & liveness probes

## CI/CD
Every commit → build → tests → scans → image → staging → production. Small, frequent releases are **safer**.

## Release strategies
| Strategy | ✅ | ❌ |
|---|---|---|
| Recreate | simple | downtime |
| **Rolling** | no downtime (k8s default) | old+new together, slow rollback |
| **Blue-green** | instant switch & rollback | double capacity |
| **Canary** (1% → 10% → 100%) | tiny blast radius | needs good metrics |

- **Feature flags:** deploy ≠ release; kill switch
- **Automated rollback** on SLO breach
- DB changes: **expand → migrate → contract**

## 🎯 Interview Line
**Q: How do you deploy without downtime and limit the damage of a bad release?**
> Behind a load balancer with readiness checks, use rolling, blue-green or canary deployments — canary sends 1% then more of traffic to the new version while watching error rate and latency, rolling back automatically if they degrade. Feature flags separate deploy from release, and database migrations are backward-compatible using expand-and-contract.

## Key words
`Container` · `Image` · `Kubernetes` · `Pod` · `Deployment` · `HPA` · `CI/CD` · `Rolling` · `Blue-green` · `Canary` · `Feature flag`
