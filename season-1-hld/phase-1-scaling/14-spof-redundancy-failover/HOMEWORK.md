# Homework · 14 · SPOF, Redundancy & Failover

Post answers in **Discussions** (thread: *S1E14 Homework*).

## 🧠 Think
1. Why is active–active harder for databases than for app servers?
2. "We have a replica, so we don't need backups." What's wrong with this?

## ✏️ Draw
Take this design: `Users → LB → 3 App servers → Redis → DB` (all in one data centre).
Circle **every SPOF** in red. Then redraw it with no SPOFs.

## 🛠️ Build (15 minutes)
Using your Nginx + 2 servers setup from video 08: stop one server while refreshing the page continuously. Does Nginx route around it? Now stop Nginx itself. What happens? (That's the SPOF.)

## 🎯 Interview
"How would you remove single points of failure from a web application? What's RPO vs RTO?"
