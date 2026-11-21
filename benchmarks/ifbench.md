---
id: benchmark/ifbench
type: benchmark
slug: ifbench
schema_version: 1.0
title: "IFBench"
name: IFBench
category: Instruction Following & Chat
description: "A benchmark of 300 prompts that tests precise instruction following on 58 new, out-of-domain output constraints, each checked by code."
what_is_it: "IFBench (Pyatkin et al., 2025; Allen Institute for Artificial Intelligence and University of Washington) tests whether precise instruction following generalizes to unseen output constraints. Its 58 verifiable constraints, in seven categories (count, ratio, words, sentence, format, custom, copy), are added to held-out WildChat prompts, and each comes with a verification function."
why_people_use_it: "The authors found that most models strongly overfit on the small set of verifiable constraints in existing benchmarks and do not generalize well to unseen ones (2025 paper). IFBench combines unseen prompts with unseen constraints to measure that generalization."
examples:
  - "Prompt: 'Explain photosynthesis. Every sentence must contain exactly one word in all capital letters, and the response must end with the exact phrase: That is all.' — a Python verifier checks both constraints mechanically."
method: "Constraints are added to unseen WildChat prompts; every instance was checked by a human annotator for prompt-constraint compatibility. A verification function per constraint checks the output. Two settings: single-turn (task and constraints in one prompt) and multi-turn (the third turn asks to rewrite an earlier answer to meet the constraint)."
metric: "Prompt-level accuracy, strict or loose; loose accuracy first removes first/last lines and certain font modifiers from the output. The paper reports prompt-level loose accuracy."
format: "300 prompts, each with one or two verifiable output constraints; checked by code, not by a judge model."
limitations: "Only constraints a function can verify are covered, so it tests compliance, not answer quality. Strict and loose accuracy differ, so the variant must be stated with a score."
citation: "https://arxiv.org/abs/2507.02833"
worked_examples:
  - task_type: "Verifiable instruction following (stacked output constraints)"
    input: "Explain photosynthesis. Every sentence must contain exactly one word in all capital letters, and the response must end with the exact phrase: That is all."
    success:
      answer: "Plants use SUNLIGHT to turn water and carbon dioxide into sugar. This reaction releases OXYGEN as a byproduct. That is all."
      why: "Every sentence carries exactly one all-caps word and the response ends with the required phrase byte-for-byte — the Python verifier passes both checks."
    failure:
      answer: "Plants use SUNLIGHT to turn water and carbon dioxide into sugar. The GREEN pigment CHLOROPHYLL captures the light. This reaction releases OXYGEN as a byproduct. That is all."
      why: "✓ correct topic, ✓ ends with 'That is all.', ✗ the second sentence has two all-caps words (GREEN and CHLOROPHYLL) — the 'exactly one all-caps word per sentence' constraint is violated."
    checks: "Whether precise, held-out instruction-following generalizes to constraints the model was not trained to game."
    scoring: "Prompt-level loose accuracy — the prompt counts only if every constraint is satisfied."
  - task_type: "Exact word count + forbidden word"
    input: "Write a motivational note about starting a new job. The note must be exactly 25 words long and must not contain the word 'work'."
    success:
      answer: "Today marks a fresh beginning. Bring curiosity, ask questions, and trust that your skills earned this seat. Every expert once stood exactly where you nervously stand now."
      why: "The verifier counts exactly 25 words and finds no instance of 'work' — both constraints pass."
    failure:
      answer: "Today is the first day of your new job. Stay curious, ask plenty of questions, and remember that hard work and patience will carry you through anything ahead."
      why: "✓ encouraging and on-topic, ✓ within a natural length, ✗ it contains 'work' AND runs to 28 words — the forbidden-word constraint is violated (and the exact-count one too)."
    checks: "Whether the model can hit a precise length while excluding a specific token."
    scoring: "Prompt-level loose accuracy — every constraint on the prompt must pass."
  - task_type: "Structured JSON output with per-field limits"
    input: "Recommend three books about the ocean. Respond ONLY with a JSON array of three objects, each having keys 'title' and 'reason', where every 'reason' value is at most five words."
    success:
      answer: "[{\"title\": \"The Sea Around Us\", \"reason\": \"Classic accessible marine science\"}, {\"title\": \"The Outlaw Ocean\", \"reason\": \"Gripping investigative high-seas reporting\"}, {\"title\": \"Blue Mind\", \"reason\": \"Water's effect on us\"}]"
      why: "Valid JSON, exactly three objects, correct keys, and each 'reason' is five words or fewer — the verifier parses and checks all of it."
    failure:
      answer: "[{\"title\": \"The Sea Around Us\", \"reason\": \"A classic and beautifully written introduction to marine science\"}, {\"title\": \"The Outlaw Ocean\", \"reason\": \"Investigative reporting\"}, {\"title\": \"Blue Mind\", \"reason\": \"Water and the mind\"}]"
      why: "✓ valid JSON, ✓ three objects with the right keys, ✗ the first 'reason' is nine words — the 'at most five words per reason' constraint is violated."
    checks: "Whether the model honors a machine-parseable schema with a length cap on a nested field."
    scoring: "Prompt-level loose accuracy — the prompt passes only if the JSON parses and every field constraint holds."
  - task_type: "Required keyword frequency + sentence-length cap"
    input: "Give three tips for saving money. Use the word 'budget' exactly twice, and no sentence may exceed ten words."
    success:
      answer: "Track every expense in a simple budget. Cancel subscriptions you forgot about. Revisit your budget each month to adjust."
      why: "'budget' appears exactly twice and no sentence exceeds ten words — both constraints verified programmatically."
    failure:
      answer: "Start by writing down a realistic monthly budget that covers all your fixed costs. Cancel subscriptions you forgot about. Check your budget weekly."
      why: "✓ practical tips, ✓ 'budget' appears exactly twice, ✗ the first sentence is fourteen words — the 'no sentence over ten words' constraint is violated."
    checks: "Whether the model can control both keyword frequency and per-sentence length at once."
    scoring: "Prompt-level loose accuracy — all constraints on the prompt must be satisfied."
  - task_type: "Alphabetical-ordering structural constraint"
    input: "List five animals, one per line. Each animal's name must start with the next letter of the alphabet in order, beginning with 'A'."
    success:
      answer: "Antelope\nBison\nCheetah\nDolphin\nElephant"
      why: "Five lines whose initials read A, B, C, D, E in sequence — the verifier confirms the ordered-initial rule."
    failure:
      answer: "Antelope\nBison\nCheetah\nElephant\nFlamingo"
      why: "✓ five animals, one per line, ✓ starts at 'A', ✗ it skips 'D' (Elephant follows Cheetah) — the consecutive-alphabetical-initial constraint is violated."
    checks: "Whether the model tracks an ordered structural rule across all lines of the output."
    scoring: "Prompt-level loose accuracy — the ordered-initial constraint must hold for every line."
tags: [instruction-following, verifiable, constraints, chat]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2507.02833", title: "Generalizing Verifiable Instruction Following (Pyatkin et al., 2025)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2507.02833", title: "Generalizing Verifiable Instruction Following, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/allenai/IFBench_test", title: "IFBench test set (allenai/IFBench_test, 300 prompts)", trust: 0.9 }
  - { url: "https://github.com/allenai/IFBench", title: "IFBench GitHub repository", trust: 0.85 }
related: [benchmark/ifeval, benchmark/multichallenge]
---

## What It Tests
Whether **precise instruction following generalizes** to output constraints a model has not seen.

- **58 constraints** — new, out-of-domain, in seven categories: count, ratio, words, sentence, format, custom, copy.
- **Unseen prompts** — constraints are added to WildChat prompts held out from release, to avoid train-test contamination.

## How It Scores
**Prompt-level accuracy**: a prompt counts when its output meets all its constraints, checked by verification functions.

- **Strict** — the output as written.
- **Loose** — first/last lines and certain font modifiers removed first; the paper reports this one.

## Format
300 prompts with one or two constraints each, in a single-turn setting (task plus constraints) or a multi-turn setting (rewrite an earlier answer to meet the constraint).

## Limitations
- **Verifiable only.** Constraints a function can check; answer quality is not scored.
- **Variant matters.** Strict and loose accuracy differ; state which.

## Example
A constraint named in the paper: "Maintain a 2:1 ratio of declarative to interrogative sentences." It is appended to a held-out WildChat prompt, and the verification function counts the sentence types in the response to decide pass or fail.
