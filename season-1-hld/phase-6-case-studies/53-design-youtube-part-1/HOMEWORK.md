# Homework · 53 · Design YouTube — Part 1: upload, transcoding, playback

Post answers in **Discussions** (thread: *S1E53 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. Why split a video into chunks before transcoding?
2. Why does a video start blurry and then become sharp?

## ✏️ Draw
Draw the 3 pipelines: upload, processing and playback.

## 🛠️ Build
Install ffmpeg and convert a short video into 2 qualities of HLS: `ffmpeg -i in.mp4 -vf scale=-2:360 -hls_time 4 out360.m3u8`. Open the .m3u8 and read it.

## 🎯 Interview
"Design YouTube's video upload and playback." Compare with the [notes](README.md).
