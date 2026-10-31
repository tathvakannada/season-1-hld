# Homework · 62 · Design a distributed key-value store

Post answers in **Discussions** (thread: *S1E62 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why does W + R > N guarantee you read the latest write?
2. Why use Merkle trees instead of comparing every key?

## ✏️ Draw
Draw a ring with 6 nodes; place 3 keys and their 3 replicas each.

## 🛠️ Build
Python: a toy consistent-hash ring with 4 nodes and 100 virtual nodes each; add a 5th node and count how many of 10,000 keys move.

## 🎯 Interview
"Design a distributed key-value store like DynamoDB." Compare with the [notes](README.md).
