---
id: benchmark/hellaswag
type: benchmark
slug: hellaswag
schema_version: 1.0
title: "HellaSwag"
name: HellaSwag
category: Knowledge & Academic Reasoning
description: "Commonsense sentence completion: pick the most plausible of four endings, where the wrong endings were chosen by Adversarial Filtering."
what_is_it: "HellaSwag was introduced by Zellers et al. (2019) as a commonsense natural language inference dataset of 70k problems. Each gives a context from an ActivityNet video caption or a WikiHow article and four possible endings. The wrong endings are machine-generated and selected by Adversarial Filtering, in which a series of discriminators keep the endings that models misclassify."
why_people_use_it: "The paper reports that the questions are easy for humans (95.6% accuracy) but were hard for the models of the time (under 50%), so it measures everyday commonsense about how a described activity continues."
examples:
  - "A woman sits at a table with a piece of clay and begins to mold it. She... (correct: rolls it flat and cuts it with a knife; distractor: eats a piece of the clay)"
method: "Four-way multiple choice: the model picks the ending it rates most likely, zero-shot or few-shot."
metric: "Accuracy (% correct)."
format: "Four-way multiple choice; zero-shot or few-shot."
limitations: "Adversarial Filtering selected endings that fooled the discriminator models of 2019, so the distractors are tuned to those models. Scores depend on the number of shots and how the ending likelihood is computed, so numbers from different harnesses are not comparable."
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
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/1905.07830", title: "HellaSwag: Can a Machine Really Finish Your Sentence? (Zellers et al., 2019)", trust: 0.95 }
  - { url: "https://ar5iv.labs.arxiv.org/html/1905.07830", title: "HellaSwag paper, full text (ar5iv)", trust: 0.95 }
related: [benchmark/arc-challenge, benchmark/winogrande]
---

## What It Tests
**Everyday physical and temporal commonsense**: knowing how a described activity plausibly continues.

## How It Scores
**Accuracy** over four-way choices. The paper reports 95.6% human accuracy.

## Format
A context from ActivityNet captions or WikiHow, then four endings; the model picks one, zero-shot or few-shot.

## Limitations
- **Dated adversary.** Distractors were filtered against 2019 models.
- **Setup-dependent.** Shot count and likelihood method change the score.

## Example
"A woman is sitting at a table with a piece of clay. She begins to mold the clay into a shape.
She..." The plausible continuation is **"rolls the clay flat and cuts it into pieces with a knife"**;
an adversarial distractor like "eats a piece of the clay" is nonsensical to a human but was designed
to look statistically plausible to weaker models.
