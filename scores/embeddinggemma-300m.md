---
id: score-sheet/embeddinggemma-300m
type: score-sheet
slug: embeddinggemma-300m
schema_version: 4.0
title: embeddinggemma-300m — scores
model: model/embeddinggemma-300m
scores:
- benchmark: benchmark/longembed
  value: 0.553212
  scale: fraction
  source: https://mteb-leaderboard.hf.space/benchmark/LongEmbed
  observed_on: '2026-11-23'
  self_reported: true
  reported_by: leaderboard
  method: 'Mean (Task) as computed and published by MTEB: the mean of the model''s main score over the 6 tasks of LongEmbed. Results are run with the open-source mteb package and submitted to the MTEB results repository, which does not record who ran them.'
- benchmark: benchmark/mteb-eng-v2
  value: 0.696736
  scale: fraction
  source: https://mteb-leaderboard.hf.space/benchmark/MTEB%28eng%2C%20v2%29
  observed_on: '2026-11-23'
  self_reported: true
  reported_by: leaderboard
  method: 'Mean (Task) as computed and published by MTEB: the mean of the model''s main score over the 41 tasks of MTEB(eng, v2). Results are run with the open-source mteb package and submitted to the MTEB results repository, which does not record who ran them.'
- benchmark: benchmark/mteb-multilingual-v2
  value: 0.61144
  scale: fraction
  source: https://mteb-leaderboard.hf.space/benchmark/MTEB%28Multilingual%2C%20v2%29
  observed_on: '2026-11-23'
  self_reported: true
  reported_by: leaderboard
  method: 'Mean (Task) as computed and published by MTEB: the mean of the model''s main score over the 131 tasks of MTEB(Multilingual, v2). Results are run with the open-source mteb package and submitted to the MTEB results repository, which does not record who ran them.'
tags:
- sentence-similarity
updated: '2026-11-23'
sources:
- url: https://mteb-leaderboard.hf.space/benchmark/MTEB%28eng%2C%20v2%29
  title: MTEB leaderboard (mteb-leaderboard.hf.space), results from the embeddings-benchmark/results repository, CC0 1.0; credit MTEB.
- url: https://mteb-leaderboard.hf.space/benchmark/MTEB%28Multilingual%2C%20v2%29
  title: MTEB leaderboard (mteb-leaderboard.hf.space), results from the embeddings-benchmark/results repository, CC0 1.0; credit MTEB.
- url: https://mteb-leaderboard.hf.space/benchmark/LongEmbed
  title: MTEB leaderboard (mteb-leaderboard.hf.space), results from the embeddings-benchmark/results repository, CC0 1.0; credit MTEB.
---


