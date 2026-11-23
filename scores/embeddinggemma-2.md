---
id: score-sheet/embeddinggemma-2
type: score-sheet
slug: embeddinggemma-2
schema_version: 2.0
title: embeddinggemma-2 — scores
model: model/embeddinggemma-2
scores:
- benchmark: benchmark/mteb-eng-v2
  value: 0.675258
  scale: fraction
  source: https://mteb-leaderboard.hf.space/benchmark/MTEB%28eng%2C%20v2%29
  observed_on: '2026-11-23'
  self_reported: true
  reported_by: leaderboard
  method: 'Mean (Task) as computed and published by MTEB: the mean of the model''s main score over the 41 tasks of MTEB(eng, v2). Results are run with the open-source mteb package and submitted to the MTEB results repository, which does not record who ran them.'
- benchmark: benchmark/mteb-multilingual-v2
  value: 0.605203
  scale: fraction
  source: https://mteb-leaderboard.hf.space/benchmark/MTEB%28Multilingual%2C%20v2%29
  observed_on: '2026-11-23'
  self_reported: true
  reported_by: leaderboard
  method: 'Mean (Task) as computed and published by MTEB: the mean of the model''s main score over the 131 tasks of MTEB(Multilingual, v2). Results are run with the open-source mteb package and submitted to the MTEB results repository, which does not record who ran them.'
tags:
- feature-extraction
- embedding-gemma2
- dense
- gqa
updated: '2026-11-23'
sources:
- url: https://mteb-leaderboard.hf.space/benchmark/MTEB%28eng%2C%20v2%29
  title: MTEB leaderboard (mteb-leaderboard.hf.space), results from the embeddings-benchmark/results repository, CC0 1.0; credit MTEB.
- url: https://mteb-leaderboard.hf.space/benchmark/MTEB%28Multilingual%2C%20v2%29
  title: MTEB leaderboard (mteb-leaderboard.hf.space), results from the embeddings-benchmark/results repository, CC0 1.0; credit MTEB.
---


