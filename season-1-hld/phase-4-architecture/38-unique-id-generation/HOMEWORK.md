# Homework · 38 · Unique ID generation at scale

Post answers in **Discussions** (thread: *S1E38 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why are random UUID v4 primary keys bad for B-tree indexes?
2. What happens to Snowflake if the server clock jumps back 2 seconds?

## ✏️ Draw
Draw the 64-bit Snowflake bar with its 4 parts and label how many values each part allows.

## 🛠️ Build
Implement a Snowflake ID generator in Python (timestamp << 22 | machine << 12 | sequence) and generate 10,000 IDs — check they're unique and sorted.

## 🎯 Interview
"Design a unique ID generator for a sharded system." Compare with the [notes](README.md).
