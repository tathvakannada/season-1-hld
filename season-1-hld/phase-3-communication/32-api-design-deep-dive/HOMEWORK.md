# Homework · 32 · API design deep dive

Post answers in **Discussions** (thread: *S1E32 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why does offset pagination show duplicates when new orders arrive while scrolling?
2. Why must the idempotency key stay the same across retries?

## ✏️ Draw
Draw the idempotency-key flow with a network failure in the middle and a retry.

## 🛠️ Build
Call the GitHub REST API for a big repo's issues (`?per_page=5&page=2`) and look at the `Link` header for pagination.

## 🎯 Interview
"Offset vs cursor pagination? REST vs gRPC vs GraphQL — when?" Compare with the [notes](README.md).
