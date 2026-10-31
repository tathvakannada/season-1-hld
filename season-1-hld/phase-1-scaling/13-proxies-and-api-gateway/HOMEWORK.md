# Homework · 13 · Proxies & API Gateway

Post answers in **Discussions** (thread: *S1E13 Homework*).

## 🧠 Think
1. Is a load balancer a reverse proxy? Explain in one line.
2. Your college Wi-Fi blocks some websites. What kind of proxy is that?

## ✏️ Draw
Add an **API gateway** to the Oota diagram: users → LB → gateway → user service / menu service / order service. Write on the gateway the 3 jobs it does for every request.

## 🛠️ Build (30 minutes, needs Docker)
Configure Nginx to route by path:
```nginx
location /api/    { proxy_pass http://api_server; }
location /images/ { proxy_pass http://image_server; }
```
Hit both paths and confirm they reach different servers.

## 🎯 Interview
"Forward proxy vs reverse proxy vs API gateway — what's the difference?"
