---
id: benchmark/winogrande
type: benchmark
slug: winogrande
schema_version: 1.0
title: "WinoGrande"
name: WinoGrande
category: Knowledge & Academic Reasoning
description: "Large-scale Winograd-style pronoun-resolution problems testing common-sense coreference."
what_is_it: "A 44,000-item benchmark of fill-in-the-blank sentences where a pronoun or blank must be resolved to the correct noun using common sense. It scales up the classic Winograd Schema Challenge and uses adversarial filtering (AfLite) to remove dataset-specific word cues."
why_people_use_it: "It targets a narrow but important skill — common-sense coreference — and was part of the original HF Open LLM Leaderboard, so historical scores are broadly available."
examples:
  - "The trophy doesn't fit into the brown suitcase because _ is too large. (correct: the trophy)"
method: "5-shot two-way choice (Open LLM v1) or 0-shot; the model picks which of two candidate fillers makes the sentence coherent."
metric: "Accuracy (% correct)."
format: "5-shot (Open LLM v1) or 0-shot, binary choice."
saturation: high
limitations: "Binary choice means a 50% random floor and a compressed dynamic range. Frontier models score in the high 80s/90s, so it is saturating. AfLite filtering reduces but does not eliminate spurious cues."
citation: "https://arxiv.org/abs/1907.10641"
worked_examples:
  - task_type: "Commonsense coreference — relative size"
    input: "Fill the blank with the correct referent: 'The trophy doesn't fit into the brown suitcase because _ is too large.'"
    options:
      - "the trophy"
      - "the suitcase"
    success:
      answer: "A — the trophy"
      why: "The object that is 'too large' to fit must be the thing being inserted — the trophy."
    failure:
      answer: "B — the suitcase"
      why: "It flips the causal relation; the suitcase is the container, and a larger container would fit the trophy better, not worse. Swapping 'large' for 'small' is the twin item whose answer flips to the suitcase."
    checks: "Resolving the referent from world knowledge and the causal cue, not surface word association."
    scoring: "Exact-match accuracy on the selected referent."
  - task_type: "Commonsense coreference — authority and motive"
    input: "Fill the blank with the correct referent: 'The city councilmen refused the demonstrators a permit because _ feared violence.'"
    options:
      - "the city councilmen"
      - "the demonstrators"
    success:
      answer: "A — the city councilmen"
      why: "The party with the authority to refuse a permit, acting out of fear of violence, is the councilmen."
    failure:
      answer: "B — the demonstrators"
      why: "Swap 'feared' for 'advocated' and the answer flips to the demonstrators; choosing them here ignores who holds the permit-granting authority and the motive cue."
    checks: "Assigning a motive to the agent whose role in the sentence makes it coherent."
    scoring: "Exact-match accuracy on the selected referent."
  - task_type: "Commonsense coreference — who needs the money"
    input: "Fill the blank with the correct referent: 'Sarah lent money to Emma because _ was saving up for a new car.'"
    options:
      - "Sarah"
      - "Emma"
    success:
      answer: "B — Emma"
      why: "The person receiving the loan is the one saving up for the car and short of funds — Emma."
    failure:
      answer: "A — Sarah"
      why: "It defaults to the grammatical subject of the main clause, ignoring that the borrower (Emma), not the lender, is the one saving up."
    checks: "Overriding a subject-of-sentence bias with the causal logic of who needs the loan."
    scoring: "Exact-match accuracy on the selected referent."
  - task_type: "Commonsense coreference — weight and crossing"
    input: "Fill the blank with the correct referent: 'The delivery truck could not cross the old bridge because _ was too heavy.'"
    options:
      - "the truck"
      - "the bridge"
    success:
      answer: "A — the truck"
      why: "The object whose weight prevents the crossing is the moving vehicle — the truck."
    failure:
      answer: "B — the bridge"
      why: "Swap 'heavy' for 'weak' and the answer flips to the bridge; choosing the bridge here misreads which object the adjective 'heavy' describes."
    checks: "Attaching the property in the clause to the physically sensible entity."
    scoring: "Exact-match accuracy on the selected referent."
  - task_type: "Commonsense coreference — spatial fit"
    input: "Fill the blank with the correct referent: 'The pen would not fit inside the case because _ was too short.'"
    options:
      - "the pen"
      - "the case"
    success:
      answer: "B — the case"
      why: "If something is too short to hold the pen, it is the container failing to fit it — the case."
    failure:
      answer: "A — the pen"
      why: "A shorter pen would fit into a case more easily, not less; picking the pen inverts the spatial relation the sentence describes."
    checks: "Reasoning about containment: a too-short container fails to hold its contents."
    scoring: "Exact-match accuracy on the selected referent."
tags: [common-sense, coreference, multiple-choice]
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/1907.10641", title: "WinoGrande: An Adversarial Winograd Schema Challenge at Scale (Sakaguchi et al., 2019)", trust: 0.95 }
related: [benchmark/hellaswag, benchmark/arc-challenge]
---

## What It Tests
Common-sense coreference: resolving an ambiguous pronoun or blank to the right entity based on world
knowledge rather than grammar. Each item comes as a twin pair that differ by one word and flip the
answer, which defeats models that latch onto surface cues.

## How It Scores
Accuracy on a binary choice. Random baseline is 50%; human performance is ~94%. The twin-pair design
means a model must get both members of a pair right to show real understanding, though the headline
metric scores items independently.

## Format
Open LLM v1 used **5-shot**; **0-shot** is also common. The task presents two candidate fillers and
the model chooses the coherent one.

## Limitations
The binary format compresses the score range and caps information per item. It is **saturating** —
top models are in the high 80s to 90s and clustered. AfLite adversarial filtering trims dataset
artifacts but cannot guarantee a model is reasoning rather than exploiting residual statistics.

## Example
"The trophy doesn't fit into the brown suitcase because _ is too large." The blank resolves to **the
trophy** (it is the large thing). Swap "large" for "small" and the answer flips to "the suitcase" —
the twin design that forces genuine reasoning.
