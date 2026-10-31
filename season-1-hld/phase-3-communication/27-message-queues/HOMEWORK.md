# Homework · 27 · Message Queues

Post answers in **Discussions** (thread: *S1E27 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why does at-least-once delivery cause duplicates?
2. What happens without a DLQ when one message has bad data?

## ✏️ Draw
Draw the spike: 5,000 msgs/min in, 1,000/min out. Sketch the queue length over the afternoon.

## 🛠️ Build
Run RabbitMQ in Docker and follow the official "Hello World" + "Work queues" tutorial (2 workers, see messages shared).

## 🎯 Interview
"Why use a queue? At-least-once vs exactly-once? What is a DLQ?" Compare with the [notes](README.md).
