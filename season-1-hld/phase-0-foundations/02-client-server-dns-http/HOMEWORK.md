# Homework · 02 · What happens when you open an app?

Post answers in **Discussions** (thread: *S1E02 Homework*).

## 🧠 Think
1. Why don't we just remember IP addresses instead of domain names? (Give at least 2 reasons.)
2. You changed Oota's server IP, but some users still reach the old server for hours. Why?

## ✏️ Draw
Draw the **full journey** of opening `oota.com` — all 8 steps, from typing the URL to seeing the page. Mark which steps can be skipped on your *second* visit (because of caching).

## 🛠️ Build (10 minutes)
1. Open a terminal and run:
   ```
   nslookup google.com
   ```
   What IP addresses do you get? Run it again for 2 other sites.
2. Open any website → press **F12** → **Network** tab → refresh the page.
   - Click one request. Find the **method**, **status code**, and one **header**.
   - How many requests did the page make in total?

## 🎯 Interview
**"What happens when you type a URL in the browser?"** Write your answer in 6 lines. Then compare it with the *Interview Line* in the [notes](README.md).
