---
id: benchmark/graphwalks
type: benchmark
slug: graphwalks
schema_version: 1.0
title: "GraphWalks"
name: GraphWalks
category: Long Context
description: "An OpenAI long-context benchmark: the prompt lists a huge directed graph as edges and the model must run a breadth-first search or find parents."
what_is_it: "GraphWalks gives a model a directed graph as a plain list of edges between hash-named nodes, then asks for either a breadth-first search (BFS) from a start node to a given depth or the parents of a node. The answer is a set of nodes. It is published as the Hugging Face dataset openai/graphwalks. Epoch AI counts 1,150 problems in two types, BFS and parents, with prompts up to about 1.75 million characters."
why_people_use_it: "It tests combining information from many positions in a very long input instead of retrieving one fact. Answers are checked by a program, so grading is cheap. Google DeepMind reports the BFS score for prompts between 256k and 1M tokens; OpenAI reports four numbers: BFS and parents, below and above 128k."
method: "Single turn: the model gets the prompt and returns the node set, with no system prompt and no tool access (Epoch AI's description). Each prompt includes worked examples, 4-shot with two of each type per Epoch. Graphs range from 44 to 69,867 edges and BFS depth from 1 to 39."
metric: "F1 over the returned node set, between 0 and 1 and usually shown as a percentage. It is built from recall and precision of the returned nodes and is not the fraction of tasks solved."
format: "Single-turn, text-only, few-shot (4-shot per Epoch; the dataset schema says 3-shot). Prompts range from 2,709 to 1,748,364 characters, median 110,284."
limitations: "All prompts and solutions are public, so contamination is possible. The data are synthetic, so a high score does not show real-world long-context skills such as codebase work. Epoch notes an earlier version had 24 wrong parent labels and less explicit BFS wording, fixed without a version bump; the Hugging Face rows carry dates from 2025 and 2026, so a score may refer to different file versions. Long BFS answers can exceed a model's output limit. Vendor tables use their own slices; Google's evaluation methodology for Gemini 4 Neon states it self-computed 200 problems between 256k and 1M tokens, so those scores are not official results."
citation: "https://huggingface.co/datasets/openai/graphwalks"
tags: [long-context, graph, reasoning, synthetic, openai]
updated: 2026-11-21
sources:
  - { url: "https://huggingface.co/datasets/openai/graphwalks", title: "openai/graphwalks — Hugging Face dataset", trust: 0.95 }
  - { url: "https://epoch.ai/benchmarks/graphwalks/review", title: "GraphWalks — benchmark review (Epoch AI)", trust: 0.9 }
  - { url: "https://storage.googleapis.com/deepmind-media/gemini/gemini_4_neon_model_evaluation.pdf", title: "Gemini 4 Neon model evaluation (Google DeepMind)", trust: 0.7 }
---

## What It Tests
Whether a model can **follow relationships across a very long input** and compute over them.

- **BFS** — nodes exactly at a given depth from a start node.
- **Parents** — all nodes with an edge into a given node.
- **Scale** — contexts from a few thousand characters up to about 1M tokens.

Epoch notes BFS needs more processing than parents, which mostly rewards retrieval.

## How It Scores
**F1** between the returned node set and the key.

- **Recall** — how many right nodes were found.
- **Precision** — how few wrong nodes were added.
- **Reading it** — F1 is not the share of tasks solved.

## Format
One turn, no tools, no system prompt, answer as a node list. The long slice holds 200 prompts at 256k and 200 at 1M tokens (Epoch's count for the above-128k subset).

## Limitations
- **Public data.** Prompts and solutions are public.
- **Synthetic.** Hash-named graphs may not reflect real tasks such as large codebases.
- **File changes.** An earlier version had 24 wrong labels, fixed without a version bump.
- **Output limits.** Long BFS answers can be longer than a model may emit.

## Example
Epoch shows the prompt shape with a tiny graph. The edges are `uvwx -> alke`, `abcd -> uvwx`, `abcd -> efgh` and `efgh -> uvwx`. The operation 'perform a BFS from node abcd with depth 1' has the answer **[uvwx, efgh]**. Asking for the parents of `uvwx` gives **[abcd, efgh]**. Real prompts use thousands of hash-named nodes instead of these four.
