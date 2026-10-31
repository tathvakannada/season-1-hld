# Homework · 49 · Design a notification system

Post answers in **Discussions** (thread: *S1E49 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why can't we guarantee exactly-once delivery with an SMS provider?
2. Why store push tokens per device, not per user?

## ✏️ Draw
Draw the pipeline with two priority lanes and four channel queues.

## 🛠️ Build
Write a Python dedupe function using a set with expiry (dict of key → expiry time). Send the same notification 3 times; only one should go through.

## 🎯 Interview
"Design a notification system that supports push, SMS and email." Compare with the [notes](README.md).
