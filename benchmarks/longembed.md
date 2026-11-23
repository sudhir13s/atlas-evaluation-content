---
id: benchmark/longembed
type: benchmark
slug: longembed
schema_version: 4.0
title: LongEmbed
name: LongEmbed
category: Embeddings & Retrieval
description: Six English retrieval tasks with long documents, two synthetic and four real, that test whether an embedding model still finds the right document when the key text is buried deep inside it.
what_is_it: LongEmbed was introduced by Zhu et al. (EMNLP 2024) to test embedding models on long inputs. Its authors found that older retrieval benchmarks have short documents, under 300 words on average in BEIR, or put the answer near the start. LongEmbed has two synthetic tasks, passkey and needle retrieval, with documents from 256 to 32,768 tokens, and four real tasks built on NarrativeQA, QMSum, 2WikiMultihopQA and SummScreenFD, whose documents average 5,582 to 50,474 words. MTEB includes it as a benchmark and publishes its results on the MTEB leaderboard.
why_people_use_it: Search over contracts, books or meeting transcripts needs an embedding model that reads a long document whole. The paper reports that the models it tested leave large room for improvement on such inputs.
examples:
- 'Passkey task, as the paper describes it: each document holds a person''s name and their passkey at a random position inside repeated filler sentences such as ''The grass is green. The sky is blue.''; the query names the person, and the model must retrieve that person''s document.'
method: MTEB embeds the queries and documents of the six tasks with the model and ranks the documents for each query by cosine similarity. Each task has one main score; the headline Mean (Task) is the mean of the six main scores, computed by MTEB. A model missing any task gets no Mean (Task). Inputs longer than a model accepts are cut to its limit. Results are run with the mteb package and submitted to the MTEB results repository.
metric: 'Mean (Task): the mean of the six task main scores, as a fraction from 0 to 1 (higher is better). The main score is nDCG@1 for the passkey and needle tasks, which have one correct document per query, and nDCG@10 for the four real tasks. The leaderboard also groups the synthetic results by context length.'
format: 'Zero-shot retrieval: the model embeds a query and the candidate documents; no answer text is generated. For the synthetic tasks, each context length has 50 queries, each with 100 candidate documents.'
limitations: 'The passkey and needle tasks are synthetic: the needle facts were written by GPT-4 and inserted into Paul Graham essays. All tasks are English. In the paper, the keyword method BM25 averaged 90.4 against 75.3 for the best embedding model tested, so a high LongEmbed score is not the best result available for these tasks. The results repository does not record who ran each model.'
citation: https://aclanthology.org/2024.emnlp-main.47/
openness: open
openness_note: The six tasks are public and ungated in the dwzhu/LongEmbed dataset on Hugging Face, whose card states no licence, and the scoring code (the Apache-2.0 mteb package) and every submitted result (the CC0 embeddings-benchmark/results repository) are public.
tags:
- embeddings
- retrieval
- long-context
- english
- mteb
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2404.12096
  title: 'LongEmbed: Extending Embedding Models for Long Context Retrieval (Zhu et al., 2024)'
  trust: 0.95
- url: https://arxiv.org/html/2404.12096v3
  title: 'LongEmbed paper, full text: tasks, sizes, passkey and needle construction, BM25 results'
  trust: 0.95
- url: https://aclanthology.org/2024.emnlp-main.47/
  title: LongEmbed in the EMNLP 2024 proceedings
  trust: 0.95
- url: https://huggingface.co/datasets/dwzhu/LongEmbed
  title: 'dwzhu/LongEmbed dataset (Hugging Face): six configurations, public, no licence stated'
  trust: 0.9
- url: https://github.com/embeddings-benchmark/mteb/blob/main/mteb/benchmarks/benchmarks/benchmarks.py
  title: 'LongEmbed definition in MTEB: its six tasks and context-length groups (embeddings-benchmark/mteb)'
  trust: 0.95
- url: https://github.com/embeddings-benchmark/mteb/blob/main/mteb/benchmarks/_create_table.py
  title: How the leaderboard computes Mean (Task) (embeddings-benchmark/mteb)
  trust: 0.95
- url: https://github.com/embeddings-benchmark/results
  title: MTEB results repository, CC0 1.0 (embeddings-benchmark/results)
  trust: 0.95
- url: https://mteb-leaderboard.hf.space/benchmark/LongEmbed
  title: 'MTEB leaderboard: LongEmbed'
  trust: 0.95
related:
- benchmark/mteb-eng-v2
- benchmark/mteb-multilingual-v2
---

## What It Tests

Whether an **embedding model finds the right long document** when the matching text sits anywhere inside it.

- **Makers** — Zhu et al., EMNLP 2024; part of the MTEB benchmark set.
- **Synthetic tasks** — passkey and needle retrieval, documents from 256 to 32,768 tokens.
- **Real tasks** — NarrativeQA, QMSum, 2WikiMultihopQA, SummScreenFD; average document length 5,582 to 50,474 words.
- **Why built** — in older benchmarks documents are short or the answer sits near the start.

## How It Scores

**Mean (Task)** = the mean of the model's main score over the six tasks, as MTEB computes it.

- **Synthetic tasks** — nDCG@1: is the one correct document ranked first?
- **Real tasks** — nDCG@10 over the top ten ranked documents.
- **Missing task** — no Mean (Task) is published for that model.

## Format

The model embeds the query and every candidate document; documents are ranked by cosine similarity.

- **Synthetic size** — 50 queries per context length, 100 candidate documents each.
- **Long inputs** — text beyond a model's limit is cut off.

## Limitations

- **Synthetic half.** Needle facts were written by GPT-4 and placed in Paul Graham essays.
- **English only.**
- **Keyword baseline.** In the paper, BM25 averaged 90.4 against 75.3 for the best embedding model tested.
- **Who ran it.** The results repository does not record who ran each model.

## Example

The passkey task, as the paper describes it (Figure 3):

- **Document** — repeated filler such as "The grass is green. The sky is blue. The sun is yellow.", with one person's name and passkey hidden at a random position.
- **Query** — asks for that person's passkey.
- **Scoring** — the person's document must be ranked first among the candidates (nDCG@1).
