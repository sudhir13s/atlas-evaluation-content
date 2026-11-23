---
id: benchmark/mteb-eng-v2
type: benchmark
slug: mteb-eng-v2
schema_version: 4.0
title: MTEB English v2
name: MTEB(eng, v2)
category: Embeddings & Retrieval
description: 41 English tasks that test text embedding models on classification, clustering, retrieval, reranking, pair classification, semantic similarity and summary scoring.
what_is_it: 'MTEB(eng, v2) is the English benchmark of the Massive Text Embedding Benchmark (MTEB), introduced in the MMTEB paper (Enevoldsen et al., 2025) as a faster version of the original English MTEB of 2022. It has 41 tasks: 10 retrieval, 9 semantic similarity, 8 classification, 8 clustering, 3 pair classification, 2 reranking and 1 summarization. The authors intend it as a zero-shot benchmark, so it leaves out datasets often used to train embedding models, such as MS MARCO and Natural Questions. MTEB runs it with its open-source mteb package and publishes the results on its leaderboard.'
why_people_use_it: An embedding model turns a text into a list of numbers, a vector, used for search, grouping and comparison. The original MTEB paper found that no single embedding method was best across all tasks, so the benchmark scores one model on many kinds of task at once.
examples:
- 'From the STSBenchmark test split (mteb/stsbenchmark-sts on Hugging Face): ''A girl is styling her hair.'' and ''A girl is brushing her hair.'', rated 2.5 for similarity on a scale of 0 to 5.'
method: 'MTEB embeds each task''s texts with the model and scores the vectors in a way fixed for each task type: logistic regression for classification, k-means for clustering, cosine similarity for retrieval, reranking, pair classification and similarity. Each task has one main score; the headline Mean (Task) is the mean of the main scores over the 41 tasks, computed by MTEB. A model missing any task gets no Mean (Task). Results are run with the mteb package and submitted to the MTEB results repository.'
metric: 'Mean (Task): the mean of the 41 task main scores, as a fraction from 0 to 1 (higher is better). Main scores by task type: accuracy (classification), V-measure (clustering), nDCG@10 (retrieval), MAP (reranking), average precision (pair classification), Spearman correlation of cosine similarity (similarity and summarization). The leaderboard also shows Mean (TaskType), the mean of the per-type means, and ranks models by Borda count.'
format: 'Zero-shot: the model only produces vectors for the texts; no prompt is graded and no task is trained on. Retrieval tasks use smaller corpora built by sampling hard negatives.'
limitations: 'Every task counts equally in Mean (Task), so task types with many tasks weigh more: the original MTEB paper notes this bias toward retrieval, classification and clustering. A result is only as complete as the tasks a model was run on, and the results repository does not record who ran each model. The leaderboard flags how many tasks a model''s own training data covered (its zero-shot share), because training on a task''s data can raise its score.'
citation: https://arxiv.org/abs/2502.13595
openness: open
openness_note: The task list, the scoring code (the Apache-2.0 mteb package) and every submitted result (the CC0 embeddings-benchmark/results repository) are public, and the task datasets are public on the Hugging Face Hub under their own licences.
tags:
- embeddings
- retrieval
- semantic-similarity
- clustering
- classification
- english
- mteb
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2502.13595
  title: 'MMTEB: Massive Multilingual Text Embedding Benchmark (Enevoldsen et al., 2025)'
  trust: 0.95
- url: https://arxiv.org/html/2502.13595v4
  title: 'MMTEB paper, full text: construction of MTEB(eng, v2) and the Borda count ranking'
  trust: 0.95
- url: https://arxiv.org/abs/2210.07316
  title: 'MTEB: Massive Text Embedding Benchmark (Muennighoff et al., 2022)'
  trust: 0.95
- url: https://ar5iv.labs.arxiv.org/html/2210.07316
  title: 'MTEB paper, full text: task types and their main metrics'
  trust: 0.95
- url: https://github.com/embeddings-benchmark/mteb/blob/main/mteb/benchmarks/benchmarks/benchmarks.py
  title: 'MTEB(eng, v2) definition: its 41 tasks (embeddings-benchmark/mteb)'
  trust: 0.95
- url: https://github.com/embeddings-benchmark/mteb/blob/main/mteb/benchmarks/_create_table.py
  title: How the leaderboard computes Mean (Task), Mean (TaskType) and the Borda rank (embeddings-benchmark/mteb)
  trust: 0.95
- url: https://github.com/embeddings-benchmark/results
  title: MTEB results repository, CC0 1.0 (embeddings-benchmark/results)
  trust: 0.95
- url: https://mteb-leaderboard.hf.space/benchmark/MTEB%28eng%2C%20v2%29
  title: 'MTEB leaderboard: MTEB(eng, v2)'
  trust: 0.95
- url: https://datasets-server.huggingface.co/rows?dataset=mteb/stsbenchmark-sts&config=default&split=test&offset=0&length=1
  title: STSBenchmark test split, first row (mteb/stsbenchmark-sts)
  trust: 0.9
related:
- benchmark/mteb-multilingual-v2
- benchmark/longembed
---

## What It Tests

How well a **text embedding model** turns English text into vectors that work for many jobs at once.

- **Makers** — the MTEB project; this version comes from the MMTEB paper (Enevoldsen et al., 2025).
- **Tasks** — 41: retrieval 10, semantic similarity 9, classification 8, clustering 8, pair classification 3, reranking 2, summarization 1.
- **Zero-shot by design** — datasets often used for training, such as MS MARCO and Natural Questions, are left out.
- **Replaces** — the 56-task English MTEB of 2022, now listed as MTEB(eng, v1).

## How It Scores

**Mean (Task)** = the mean of the model's main score over all 41 tasks, as MTEB computes it.

- **Main score per task type** — accuracy, V-measure, nDCG@10, MAP, average precision, or Spearman correlation.
- **Missing task** — no Mean (Task) is published for that model.
- **Also shown** — Mean (TaskType), one score per task type, and a rank by Borda count.

## Format

The model only embeds texts; MTEB's code does the rest.

- **Classification** — a logistic regression classifier is trained on the vectors.
- **Clustering** — k-means groups the vectors; V-measure checks the groups.
- **Retrieval, reranking, similarity** — texts are compared by cosine similarity.

## Limitations

- **Uneven weights.** Types with more tasks count for more in the mean, as the 2022 paper notes.
- **Who ran it.** The results repository does not record who ran each model.
- **Training overlap.** A model trained on a task's data can score higher; the leaderboard shows each model's zero-shot share.

## Example

From the STSBenchmark test split on Hugging Face (`mteb/stsbenchmark-sts`), first row:

- **Pair** — "A girl is styling her hair." and "A girl is brushing her hair."
- **Human rating** — 2.5 on a scale of 0 to 5.
- **Scoring** — the model's cosine similarity for every pair is compared with the human ratings by Spearman correlation.
