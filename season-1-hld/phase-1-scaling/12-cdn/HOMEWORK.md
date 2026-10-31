# Homework · 12 · CDN

Post answers in **Discussions** (thread: *S1E12 Homework*).

## 🧠 Think
1. What should **not** go on a CDN? Give 2 examples.
2. You cached `logo.png` on the CDN for 1 year, and now you've changed the logo. What are your options?

## ✏️ Draw
Draw the flow for a **cache miss** and a **cache hit**: user → nearest edge → origin. Mark where the time is saved.

## 🛠️ Build (10 minutes)
Open a big website → **F12** → **Network** → click an image or JS file → **Response Headers**. Look for headers like:
`cache-control`, `age`, `x-cache`, `cf-cache-status`, `via`.
Which CDN do you think the site uses? Was it a HIT or a MISS?

## 🎯 Interview
"How does a CDN reduce latency? How would you update a file that's cached for a year?"
