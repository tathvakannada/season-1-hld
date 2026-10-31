# Homework · 24 · Storage types: block, file, object

Post answers in **Discussions** (thread: *S1E24 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why not store images as BLOBs in the database?
2. Why use presigned URLs instead of uploading through your app servers?

## ✏️ Draw
Draw the full photo upload + view flow: app → backend → presigned URL → S3 → CDN → user.

## 🛠️ Build
Run MinIO (S3-compatible) in Docker, create a bucket, upload a file from the web console, and open it by URL.

## 🎯 Interview
"Block vs file vs object storage? How would you design photo uploads for Oota?" Compare with the [notes](README.md).
