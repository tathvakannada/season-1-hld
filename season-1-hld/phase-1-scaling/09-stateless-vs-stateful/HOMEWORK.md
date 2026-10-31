# Homework · 09 · Stateless vs Stateful servers

Post answers in **Discussions** (thread: *S1E09 Homework*).

## 🧠 Think
1. Why do sticky sessions cause trouble when a server crashes or when autoscaling removes servers?
2. If JWTs can't easily be revoked, how would you log a user out "everywhere"?

## ✏️ Draw
Redraw Oota with a **Redis session store**: LB → 3 app servers → Redis + DB. Show a user's 3 requests going to 3 different servers and still staying logged in.

## 🛠️ Build (30 minutes)
Make a tiny app that keeps a **visit counter in memory**, run 2 copies behind Nginx (from video 08), and refresh many times. The counter jumps around — why?
Bonus: store the counter in Redis (`INCR visits`) and see it become correct.

## 🎯 Interview
"How do you make a web tier stateless? Session store vs JWT — pros and cons?"
