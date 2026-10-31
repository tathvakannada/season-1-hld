# Homework · 08 · Load Balancing

Post answers in **Discussions** (thread: *S1E08 Homework*).

## 🧠 Think
1. Which algorithm would you choose if some servers are 2× bigger than others? Why?
2. Which algorithm for long-lived WebSocket connections? Why not round robin?

## ✏️ Draw
Draw: users → **2 load balancers** (active–passive) → 3 app servers with health checks. Mark what happens when App 2 dies, and when the active LB dies.

## 🛠️ Build (30 minutes, needs Docker)
Put **Nginx** in front of two tiny servers that reply "I am server 1" / "I am server 2":
```nginx
upstream oota { server app1:80; server app2:80; }
server { listen 80; location / { proxy_pass http://oota; } }
```
Refresh the browser several times — watch the responses alternate. Then stop one server and refresh again.

## 🎯 Interview
"How does a load balancer know a server is down, and how do you stop the load balancer from being a single point of failure?"
