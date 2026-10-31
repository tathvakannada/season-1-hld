# Homework · 52 · Design a news feed (Instagram)

Post answers in **Discussions** (thread: *S1E52 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why store only post IDs in the feed cache?
2. Why skip fan-out for users who haven't opened the app in 30 days?

## ✏️ Draw
Draw both paths: a normal user posting and a celebrity posting, and how your feed gets both.

## 🛠️ Build
Use Redis lists: LPUSH post IDs into 3 users' feeds, LTRIM to 5, LRANGE to read. Simulate one celebrity post merged at read time in Python.

## 🎯 Interview
"Design Instagram's home feed." Compare with the [notes](README.md).
