---
id: benchmark/graphwalks
type: benchmark
slug: graphwalks
schema_version: 4.0
title: GraphWalks
name: GraphWalks
category: Long Context
description: 'An OpenAI long-context benchmark: the prompt lists a huge directed graph as edges and the model must run a breadth-first search or find parents.'
what_is_it: GraphWalks is an OpenAI dataset, first published in April 2025 and introduced with GPT-4.1, for multi-hop reasoning over long contexts. The prompt fills the context with a directed graph written as edges between hash-named nodes, then asks for either a breadth-first search (BFS) from a node to a given depth or the parents of a node. The answer is a set of nodes. The Hugging Face dataset openai/graphwalks has 1,150 problems of two types, bfs and parents, with prompts from about 2.6 thousand to 1.75 million characters.
why_people_use_it: 'OpenAI designed it so the task cannot be solved by reading the prompt once from start to end: the model has to combine edges scattered across the whole input. Answers are checked by a program, so grading is cheap. OpenAI reports four numbers: BFS and parents, each below and above 128k tokens.'
method: 'Single turn: the whole prompt is sent as one user message. It opens with the BFS and parents rules and three worked examples (3-shot), then the graph and the operation. The model must end with a line ''Final Answer: [node, node, ...]'', which a script parses and compares as a set with the expected nodes.'
metric: F1 over the returned node set, between 0 and 1 and usually shown as a percentage. Recall = correct nodes returned / expected nodes; precision = correct nodes returned / nodes returned; F1 is their harmonic mean. A parse failure scores 0, and an empty answer to an empty key scores 1.
format: Single-turn, text-only, 3-shot. Prompts range from about 2.6 thousand to 1.75 million characters; expected answers hold from 0 to about 6,900 nodes.
limitations: All prompts and solutions are public, so contamination is possible. The graphs are synthetic, so a high score does not show real-world long-context skills such as codebase work. A 27 February 2026 bug fix corrected 24 of 400 parents samples in the 128k-and-shorter file that wrongly included the root node, and clarified the BFS wording, without a new version name; rows carry the dates 2025-04-12 or 2026-02-27, so older scores may refer to the earlier files. Long BFS answers can exceed a model's output limit. Vendor tables use their own length slices, so check which slice a score covers.
citation: https://huggingface.co/datasets/openai/graphwalks
openness: open
openness_note: All 1,150 prompts and expected node sets are public on Hugging Face under the MIT licence, and the dataset card spells out the answer-parsing and F1 grading rules.
tags:
- long-context
- graph
- reasoning
- synthetic
- openai
updated: '2026-11-23'
sources:
- url: https://huggingface.co/datasets/openai/graphwalks
  title: openai/graphwalks — Hugging Face dataset card (MIT)
  trust: 0.9
- url: https://openai.com/index/gpt-4-1/
  title: Introducing GPT-4.1 in the API (OpenAI, Graphwalks section)
  trust: 0.95
---

## What It Tests

Whether a model can **follow relationships across a very long input** and compute over them.

- **BFS** — return the nodes at exactly a given depth from a start node, not the start node itself.
- **Parents** — return all nodes with an edge into a given node.
- **Scale** — prompts from a few thousand characters up to about 1.75 million.

**Who built it:** OpenAI; the dataset card dates the first publication 12 April 2025, and OpenAI introduced it in its GPT-4.1 announcement. A **breadth-first search** explores a graph level by level: first the direct neighbours, then their neighbours, and so on.

## How It Scores

**F1** between the returned node set and the key, computed per problem.

- **Recall** — correct nodes returned / expected nodes.
- **Precision** — correct nodes returned / nodes returned.
- **F1** — the harmonic mean of the two, so both missing and extra nodes cost points.
- **Parsing** — only the last line counts; without "Final Answer:" and a bracketed list, the problem scores 0.
- **Reading it** — F1 is not the share of problems solved.

## Format

One user message, answer as a bracketed node list on the last line.

- **Size** — 1,150 rows in one split, Parquet files.
- **Fields** — `prompt`, `answer_nodes`, `prompt_chars`, `problem_type` (`bfs` or `parents`) and `date_added`.
- **Slices** — OpenAI reports below and above 128k tokens separately.

## Limitations

- **Public data.** Prompts and solutions are public.
- **Synthetic.** Hash-named graphs may not reflect real tasks such as large codebases.
- **Silent fix.** The 2026 bug fix changed 24 labels and the BFS wording without a new version name.
- **Output limits.** Long BFS answers can be longer than a model may emit.

## Example

From the prompt shape on the dataset card (MIT licence), a tiny graph:

- **Edges** — `uvwx -> alke`, `abcd -> uvwx`, `abcd -> efgh`, `efgh -> uvwx`.
- **Operation** — "Perform a BFS from node abcd with depth 1."
- **Final Answer** — **[uvwx, efgh]**.

Under the card's parents rule, the same edges make the parents of `uvwx` **[abcd, efgh]**. Real prompts use thousands of hash-named nodes instead of these four.
