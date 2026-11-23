---
id: benchmark/mteb-multilingual-v2
type: benchmark
slug: mteb-multilingual-v2
schema_version: 4.0
title: MTEB Multilingual v2
name: MTEB(Multilingual, v2)
category: Embeddings & Retrieval
description: 131 tasks in more than 250 languages that test text embedding models on bitext mining, classification, clustering, retrieval, reranking, pair classification and semantic similarity.
what_is_it: 'MTEB(Multilingual, v2) is the multilingual benchmark of the Massive Multilingual Text Embedding Benchmark (MMTEB, Enevoldsen et al., 2025), a community-built expansion of MTEB with more than 500 tasks. The authors started from all MMTEB tasks, removed machine-translated datasets, datasets with unclear licences and highly specialised ones such as code retrieval, then removed the tasks whose results the others predicted best. The result is 131 tasks: 43 classification, 18 retrieval, 16 clustering, 16 semantic similarity, 13 bitext mining, 11 pair classification, 6 reranking, 5 multi-label classification and 3 instruction reranking. Version 2 replaced version 1 after one clustering dataset was removed from the Hugging Face Hub.'
why_people_use_it: It scores one embedding model across many languages and kinds of task at once, including languages with few speakers or little training data, which English-only benchmarks do not cover.
examples:
- 'From the Tatoeba bitext mining task (mteb/tatoeba-bitext-mining, German-English test split): ''Maria sagte, sie wisse nicht, wo Tom sei.'' is the translation to find for ''Mary said she didn''t know where Tom was.'''
method: MTEB embeds each task's texts with the model and scores the vectors in a way fixed for each task type, for example a logistic regression classifier for classification and cosine similarity for retrieval and bitext mining. Each task has one main score; the headline Mean (Task) is the mean of the main scores over the 131 tasks, computed by MTEB. A model missing any task gets no Mean (Task). Results are run with the mteb package and submitted to the MTEB results repository.
metric: 'Mean (Task): the mean of the 131 task main scores, as a fraction from 0 to 1 (higher is better). Main scores include F1 (bitext mining), accuracy (classification), V-measure (clustering), nDCG@10 (retrieval) and Spearman correlation of cosine similarity (semantic similarity). The leaderboard also shows Mean (TaskType), the mean of the per-type means, and ranks models by Borda count.'
format: 'Zero-shot: the model only produces vectors for the texts; no prompt is graded. Retrieval tasks use smaller corpora built by sampling hard negatives, and clustering reuses encoded documents across sets, to cut the cost of a run.'
limitations: Every task counts equally in Mean (Task), so classification, with 43 of the 131 tasks, weighs most. Running all 131 tasks is costly, and a model missing even one task has no published Mean (Task) and appears only in the Borda rank. The results repository does not record who ran each model.
citation: https://arxiv.org/abs/2502.13595
openness: open
openness_note: The task list, the scoring code (the Apache-2.0 mteb package) and every submitted result (the CC0 embeddings-benchmark/results repository) are public, and the task datasets are public on the Hugging Face Hub under their own licences.
tags:
- embeddings
- retrieval
- multilingual
- bitext-mining
- classification
- mteb
- mmteb
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2502.13595
  title: 'MMTEB: Massive Multilingual Text Embedding Benchmark (Enevoldsen et al., 2025)'
  trust: 0.95
- url: https://arxiv.org/html/2502.13595v4
  title: 'MMTEB paper, full text: construction of MTEB(Multilingual) and the Borda count ranking'
  trust: 0.95
- url: https://ar5iv.labs.arxiv.org/html/2210.07316
  title: 'MTEB paper, full text: task types and their main metrics (Muennighoff et al., 2022)'
  trust: 0.95
- url: https://github.com/embeddings-benchmark/mteb/blob/main/mteb/benchmarks/benchmarks/benchmarks.py
  title: MTEB(Multilingual, v1) and v2 definitions (embeddings-benchmark/mteb)
  trust: 0.95
- url: https://github.com/embeddings-benchmark/mteb/blob/main/mteb/benchmarks/_create_table.py
  title: How the leaderboard computes Mean (Task), Mean (TaskType) and the Borda rank (embeddings-benchmark/mteb)
  trust: 0.95
- url: https://github.com/embeddings-benchmark/results
  title: MTEB results repository, CC0 1.0 (embeddings-benchmark/results)
  trust: 0.95
- url: https://mteb-leaderboard.hf.space/benchmark/MTEB%28Multilingual%2C%20v2%29
  title: 'MTEB leaderboard: MTEB(Multilingual, v2)'
  trust: 0.95
- url: https://datasets-server.huggingface.co/rows?dataset=mteb/tatoeba-bitext-mining&config=deu-eng&split=test&offset=0&length=1
  title: Tatoeba bitext mining, German-English test split, first row (mteb/tatoeba-bitext-mining, CC BY 2.0)
  trust: 0.9
related:
- benchmark/mteb-eng-v2
- benchmark/longembed
---

## What It Tests

How well a **text embedding model** works across **more than 250 languages** and many kinds of task.

- **Makers** — the MMTEB community project (Enevoldsen et al., 2025), built on MTEB.
- **Tasks** — 131: classification 43, retrieval 18, clustering 16, semantic similarity 16, bitext mining 13, pair classification 11, reranking 6, multi-label classification 5, instruction reranking 3.
- **Selection** — no machine-translated data, no datasets with unclear licences; the tasks other tasks predict best were removed.
- **Version 2** — replaced version 1 after one clustering dataset left the Hugging Face Hub.

## How It Scores

**Mean (Task)** = the mean of the model's main score over all 131 tasks, as MTEB computes it.

- **Main score per task type** — for example F1 for bitext mining, accuracy for classification, nDCG@10 for retrieval.
- **Missing task** — no Mean (Task) is published for that model.
- **Also shown** — Mean (TaskType), one score per task type, and a rank by Borda count.

## Format

The model only embeds texts; MTEB's code does the rest.

- **Bitext mining** — for each sentence, the closest sentence in the other language by cosine similarity must be its translation.
- **Classification** — a logistic regression classifier is trained on the vectors.
- **Cheaper runs** — smaller retrieval corpora built from hard negatives.

## Limitations

- **Uneven weights.** Classification is a third of the tasks, so it weighs most in the mean.
- **All or nothing.** One missing task means no published Mean (Task).
- **Who ran it.** The results repository does not record who ran each model.

## Example

From the Tatoeba bitext mining task on Hugging Face (`mteb/tatoeba-bitext-mining`, German-English test split, CC BY 2.0), first row:

- **English** — "Mary said she didn't know where Tom was."
- **German** — "Maria sagte, sie wisse nicht, wo Tom sei."
- **Scoring** — the German sentence must be the closest of the 1,000 candidates by cosine similarity; F1 over all pairs is the task's main score.
