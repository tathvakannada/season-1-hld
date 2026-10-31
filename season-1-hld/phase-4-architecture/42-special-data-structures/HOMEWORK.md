# Homework · 42 · Special data structures: Bloom filter, HyperLogLog, Geohash, Trie

Post answers in **Discussions** (thread: *S1E42 Homework*). Answers to 🧠 are discussed at the start of the next video.

## 🧠 Think
1. A Bloom filter says "maybe". What do you do next?
2. Why search the 8 neighbouring geohash cells too?

## ✏️ Draw
Draw a trie for: biryani, bisi bele bath, bonda, burger. Mark top suggestions for "b" and "bi".

## 🛠️ Build
In Redis: `PFADD visitors u1 u2 u3 u1` then `PFCOUNT visitors`; and `GEOADD` 3 restaurants, then `GEOSEARCH … BYRADIUS 3 km`.

## 🎯 Interview
"How would you check if a username is taken at scale? How does autocomplete work?" Compare with the [notes](README.md).
