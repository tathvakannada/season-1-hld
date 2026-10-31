# Homework · 55 · Design ride hailing (Uber / Rapido / Namma Yatri)

Post answers in **Discussions** (thread: *S1E55 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why only keep the latest location for matching?
2. Why search neighbouring cells too?

## ✏️ Draw
Draw a city grid, a rider in one cell and 6 drivers around; show which ones are searched.

## 🛠️ Build
Use Redis GEO: GEOADD 5 driver locations in Bengaluru, then GEOSEARCH from Majestic within 3 km.

## 🎯 Interview
"Design Uber's driver matching." Compare with the [notes](README.md).
