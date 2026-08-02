---
id: benchmark/hellaswag
type: benchmark
slug: hellaswag
schema_version: 1.0
title: "HellaSwag"
name: HellaSwag
category: Knowledge & Academic Reasoning
description: "Common-sense sentence completion with adversarially filtered wrong endings."
what_is_it: "A sentence-completion test: given a short context (often from ActivityNet or WikiHow), the model picks the most plausible continuation from four options. The wrong endings are generated and adversarially filtered to fool weaker models while staying obviously wrong to humans."
why_people_use_it: "It was a standard common-sense benchmark on the original HF Open LLM Leaderboard, giving broad historical coverage. It isolates physical/temporal common sense rather than academic knowledge."
examples:
  - "A woman sits at a table with a piece of clay and begins to mold it. She... (correct: rolls it flat and cuts it with a knife; distractor: eats a piece of the clay)"
method: "10-shot multiple-choice was the HF Open LLM v1 protocol; 0-shot is also reported. Score = highest-likelihood ending among 4."
metric: "Accuracy (% correct)."
format: "10-shot (Open LLM v1) or 0-shot, 4-way multiple-choice."
saturation: high
limitations: "Near-saturated: frontier models exceed 95%, matching or beating the ~95.6% human baseline, so it no longer separates strong models. The adversarial filtering was tuned against 2019-era models and does not hold against modern ones. A few items have noisy labels."
citation: "https://arxiv.org/abs/1905.07830"
worked_examples:
  - task_type: "Commonsense completion — working with clay"
    input: "A woman sits at a table with a lump of clay and begins to mold it with her hands. She..."
    options:
      - "rolls it flat and cuts it into strips with a knife."
      - "sets the wet, unshaped clay into a hot kiln and instantly lifts out a finished, glazed pot."
      - "folds the wooden table in half and slips it into her pocket."
      - "turns into a clay statue herself."
    success:
      answer: "A — rolls it flat and cuts it into strips with a knife."
      why: "It follows the physical activity — a real next step in working soft clay by hand."
    failure:
      answer: "B — sets the wet, unshaped clay into a hot kiln and instantly lifts out a finished, glazed pot."
      why: "It shares pottery vocabulary and reads fluently, but violates process and timing — wet clay is shaped and dried before firing, and firing is not instant. This is the adversarially-filtered distractor built to look statistically plausible to weaker models."
    checks: "Rejecting a topically-fluent but physically impossible continuation using everyday process knowledge, not word overlap."
    scoring: "Exact-match accuracy on the selected ending (highest-likelihood option)."
  - task_type: "Commonsense completion — cooking eggs"
    input: "A man cracks two eggs into a bowl and starts whisking them with a fork. He..."
    options:
      - "pours the beaten eggs into a heated, oiled pan and stirs them as they set."
      - "keeps whisking until the raw eggs harden into a solid rubber ball in the bowl."
      - "puts the whole bowl of raw egg into the freezer and serves it as an omelette."
      - "eats the fork and swallows the whisk."
    success:
      answer: "A — pours the beaten eggs into a heated, oiled pan and stirs them as they set."
      why: "It continues the cooking sequence in the physically correct order — beat the eggs, then cook them."
    failure:
      answer: "B — keeps whisking until the raw eggs harden into a solid rubber ball in the bowl."
      why: "It reuses the whisking action and cooking vocabulary but is physically wrong — whisking alone never solidifies raw eggs; heat does. The adversarial distractor for weaker models."
    checks: "Using process knowledge (heat, not agitation, cooks eggs) to reject a fluent but impossible step."
    scoring: "Exact-match accuracy on the selected ending (highest-likelihood option)."
  - task_type: "Commonsense completion — shoveling snow"
    input: "A boy stands on a snow-covered driveway holding a shovel. He..."
    options:
      - "pushes the shovel forward to scrape up the snow, then tosses it onto the lawn."
      - "scoops the snow and it immediately turns into green grass under the blade."
      - "lifts the entire driveway and shakes the snow off it like a rug."
      - "eats the shovel one bite at a time until the snow melts on its own."
    success:
      answer: "A — pushes the shovel forward to scrape up the snow, then tosses it onto the lawn."
      why: "It is the ordinary, physically sensible next action for someone shoveling a driveway."
    failure:
      answer: "B — scoops the snow and it immediately turns into green grass under the blade."
      why: "It stays on-topic (snow, driveway, blade) and reads smoothly, but violates everyday causality — shoveling does not grow grass; the statistically-plausible-but-impossible distractor."
    checks: "Preferring a mundane physical continuation over a topically-related but causally impossible one."
    scoring: "Exact-match accuracy on the selected ending (highest-likelihood option)."
  - task_type: "Commonsense completion — brushing teeth"
    input: "A woman squeezes toothpaste onto a toothbrush and raises it toward her mouth. She..."
    options:
      - "brushes her teeth in small circles, then spits and rinses with water."
      - "paints the bathroom walls with the toothpaste until the tube is empty."
      - "swallows the toothbrush whole and instantly grows a new set of teeth."
      - "drops the toothbrush into a toaster and waits for it to pop up."
    success:
      answer: "A — brushes her teeth in small circles, then spits and rinses with water."
      why: "It completes the goal implied by the setup — applying toothpaste is the prelude to brushing."
    failure:
      answer: "B — paints the bathroom walls with the toothpaste until the tube is empty."
      why: "It keeps the toothpaste object but abandons the obvious goal of brushing for an implausible action — surface-coherent yet goal-incoherent, the trap for shallow models."
    checks: "Inferring the intended goal from the setup rather than latching onto a shared object noun."
    scoring: "Exact-match accuracy on the selected ending (highest-likelihood option)."
  - task_type: "Commonsense completion — washing a car"
    input: "A man dips a sponge into a bucket of soapy water and wrings it out beside his car. He..."
    options:
      - "wipes the sponge across the hood in long strokes, working the soap over the paint."
      - "inflates the car with the sponge until it floats up into the sky."
      - "pours the bucket into the engine and the car drives itself away."
      - "folds the car into a small paper square and mails it to himself."
    success:
      answer: "A — wipes the sponge across the hood in long strokes, working the soap over the paint."
      why: "It is the natural continuation of washing a car with a soapy sponge."
    failure:
      answer: "B — inflates the car with the sponge until it floats up into the sky."
      why: "It stays in the car-washing scene and reads fluently, but the outcome is physically impossible — the kind of adversarially-filtered ending built to fool weaker models."
    checks: "Choosing a physically possible action over a fluent, on-theme, but impossible one."
    scoring: "Exact-match accuracy on the selected ending (highest-likelihood option)."
tags: [common-sense, multiple-choice, completion]
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/1905.07830", title: "HellaSwag: Can a Machine Really Finish Your Sentence? (Zellers et al., 2019)", trust: 0.95 }
related: [benchmark/arc-challenge, benchmark/winogrande]
---

## What It Tests
Everyday physical and temporal common sense — knowing how a described activity plausibly continues.
The task is trivial for humans but was hard for pre-2020 models, which is exactly the gap it was
built to expose.

## How It Scores
Accuracy over the validation set. Human baseline is ~95.6%. Random baseline is 25%. Because the set
is large, scores are stable, but the ceiling is so close that top models are indistinguishable.

## Format
The HF Open LLM Leaderboard v1 used **10-shot**; papers also report **0-shot**. As with other
completion benchmarks, base models are scored by ending likelihood.

## Limitations
It is **saturated**. The adversarial-filtering trick (Adversarial Filtering) was calibrated against
2019 models, so its distractors are no longer adversarial to current systems. Use it as a smoke test
for weak common sense, not as a frontier discriminator.

## Example
"A woman is sitting at a table with a piece of clay. She begins to mold the clay into a shape.
She..." The plausible continuation is **"rolls the clay flat and cuts it into pieces with a knife"**;
an adversarial distractor like "eats a piece of the clay" is nonsensical to a human but was designed
to look statistically plausible to weaker models.
