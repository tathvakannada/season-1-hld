# Homework · 61 · Design a payment system (UPI-style)

Post answers in **Discussions** (thread: *S1E61 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why store money as integers in paise, not as 500.00 floats?
2. A payment request timed out. Why must you not mark it as failed?

## ✏️ Draw
Draw the double-entry ledger lines for: customer pays ₹500, restaurant gets ₹450, Oota keeps ₹50.

## 🛠️ Build
Python: implement `pay(order_id, amount)` with an idempotency dict; call it 3 times with the same order_id and show only one charge.

## 🎯 Interview
"Design a payment system like Razorpay/UPI checkout." Compare with the [notes](README.md).
