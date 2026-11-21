---
id: benchmark/winogrande
type: benchmark
slug: winogrande
schema_version: 1.0
title: "WinoGrande"
name: WinoGrande
category: Knowledge & Academic Reasoning
description: "44k fill-in-the-blank problems in the style of the Winograd Schema Challenge, where the right referent of a blank needs commonsense."
what_is_it: "WinoGrande was introduced by Sakaguchi et al. (2019) as a large-scale dataset of 44k problems inspired by the Winograd Schema Challenge (WSC), adjusted to improve scale and hardness. Problems were crowdsourced as twin sentences that differ slightly and flip the answer, then filtered with AfLite, an algorithm that removes problems with machine-detectable embedding associations. Each problem has two options for the blank. In the paper, human performance is 94.0%."
why_people_use_it: "The authors built it to test whether models have robust commonsense or rely on dataset-specific biases, which they argue led to overestimating machine commonsense on earlier benchmarks."
examples:
  - "The trophy doesn't fit into the brown suitcase because _ is too large. (correct: the trophy)"
method: "The model chooses which of the two options fills the blank, zero-shot or few-shot."
metric: "Accuracy (% correct)."
format: "Two-option fill-in-the-blank; zero-shot or few-shot."
limitations: "With two options, random guessing scores 50%, which compresses the useful range. The paper argues that algorithmic bias reduction such as AfLite mitigates, but human-written problems still contain annotation artifacts. Scores depend on the number of shots and the scoring method."
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
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/1907.10641", title: "WinoGrande: An Adversarial Winograd Schema Challenge at Scale (Sakaguchi et al., 2019)", trust: 0.95 }
  - { url: "https://ar5iv.labs.arxiv.org/html/1907.10641", title: "WinoGrande paper, full text (ar5iv)", trust: 0.95 }
related: [benchmark/hellaswag, benchmark/arc-challenge]
---

## What It Tests
**Commonsense coreference**: filling a blank with the right entity from world knowledge, not grammar.

## How It Scores
**Accuracy** on a two-option choice. Random guessing scores 50%; the paper reports 94.0% for humans.

## Format
A sentence with a blank and two candidate fillers. Problems come as twin sentences that flip the answer.

## Limitations
- **Binary.** A 50% floor compresses the range.
- **Residual artifacts.** AfLite reduces but does not remove dataset bias.
- **Setup-dependent.** Shot count and scoring change the number.

## Example
"The trophy doesn't fit into the brown suitcase because _ is too large." The blank resolves to **the
trophy** (it is the large thing). Swap "large" for "small" and the answer flips to "the suitcase" —
the twin design that forces genuine reasoning.
