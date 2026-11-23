---
id: benchmark/ifeval
type: benchmark
slug: ifeval
schema_version: 3.1
title: "IFEval"
name: IFEval
category: Instruction Following & Chat
description: "Instruction-Following Eval: 541 prompts carrying verifiable instructions, such as a minimum word count, that a program can check."
what_is_it: "IFEval was introduced by Zhou et al. (arXiv, November 2023), with code in the google-research repository and the data on Hugging Face under the Apache 2.0 licence. It focuses on \"verifiable instructions\" such as \"write in more than 400 words\" or \"mention the keyword of AI at least 3 times\": the authors identified 25 types of them and built 541 prompts, each with one or more instructions."
why_people_use_it: "The authors note that human evaluation is expensive, slow and not reproducible, and that model-based evaluation can be biased by the evaluator; IFEval checks each instruction with code instead."
examples:
  - "From the Hugging Face dataset (key 1001, Apache 2.0): I am planning a trip to Japan, and I would like thee to write an itinerary for my journey in a Shakespearean style. ... (instruction checked: punctuation:no_comma)"
method: "The model answers each prompt; a verification function per instruction type checks the response. A loose variant also accepts responses that pass after simple transformations, which the authors treat as a complement to the strict check."
metric: "Four accuracies: prompt-level strict = prompts where every instruction is followed / prompts; instruction-level strict = instructions followed / instructions; and the loose versions of both."
format: "0-shot; programmatic verification."
limitations: "It checks only whether verifiable instructions are followed, not whether the answer is good. The loose criterion reduces false negatives but can introduce false positives, so strict and loose numbers must not be mixed. The 25 instruction types do not cover every kind of instruction."
citation: "https://arxiv.org/abs/2311.07911"
openness: open
openness_note: "All 541 prompts with their instruction lists are public on Hugging Face under Apache 2.0, and the verification code is public in the google-research repository."
worked_examples:
  - task_type: "Verifiable instruction following (format + forbidden word)"
    input: "Write a summary of the water cycle in exactly two paragraphs, and do not use the word 'water'."
    success:
      answer: "Moisture evaporates from oceans and lakes, rising as vapor that cools and condenses into clouds.\n\nThose clouds release rain and snow, which flows across land and returns to the sea, restarting the cycle."
      why: "Exactly two paragraphs and the forbidden word never appears — both deterministic verifiers pass."
    failure:
      answer: "Water evaporates from oceans and lakes, rising as vapor that condenses into clouds.\n\nThe clouds release precipitation, which flows across land and returns to the sea to begin again."
      why: "✓ exactly two paragraphs, ✓ accurate and well-written, ✗ the first sentence uses the forbidden word 'Water' — the 'do not use the word water' constraint is violated."
    checks: "Literal adherence to explicit, machine-checkable output constraints, independent of answer quality."
    scoring: "Instruction-following accuracy — fraction of constraints satisfied (prompt-level counts the prompt only if all pass)."
  - task_type: "Case constraint (all lowercase)"
    input: "Explain what a firewall does in two to three sentences. Your entire response must be in English and in all lowercase letters — no capital letters are allowed anywhere."
    success:
      answer: "a firewall monitors incoming and outgoing network traffic and decides what to allow or block. it enforces a set of security rules, keeping untrusted connections away from your devices. think of it as a gatekeeper between your network and the internet."
      why: "Accurate, appropriately brief, and the verifier confirms not a single uppercase character appears — the case constraint passes."
    failure:
      answer: "a firewall monitors network traffic and decides what to allow or block based on security rules. it acts as a barrier between a trusted internal network and untrusted outside networks like the Internet."
      why: "✓ correct and concise, ✓ mostly lowercase, ✗ 'Internet' is capitalized — the 'all lowercase, no capital letters' constraint is violated."
    checks: "Whether the model applies a global character-case rule across the whole response."
    scoring: "Instruction-following accuracy — the lowercase verifier scans every character; one uppercase letter fails the prompt."
  - task_type: "Exact number of bullet points"
    input: "Give me tips for a good night's sleep. Respond with exactly three bullet points, each starting with a markdown '* ' marker, and nothing else."
    success:
      answer: "* Keep a consistent sleep and wake time, even on weekends.\n* Avoid screens and bright light for an hour before bed.\n* Keep the bedroom cool, dark, and quiet."
      why: "Exactly three markdown bullets and no surrounding text — the bullet-count verifier passes."
    failure:
      answer: "Here are some tips for better sleep:\n* Keep a consistent sleep and wake time.\n* Avoid screens before bed.\n* Keep the bedroom cool and dark.\n* Limit caffeine in the afternoon."
      why: "✓ helpful, ✓ correct bullet format, ✗ it emits four bullets plus a lead-in line — the 'exactly three bullet points' constraint is violated."
    checks: "Whether the model produces a precise, counted list structure with no extra content."
    scoring: "Instruction-following accuracy — the verifier counts bullet markers; anything other than three fails the prompt."
  - task_type: "Wrapped JSON output"
    input: "What is the capital of France, and roughly how many people live there? Wrap your entire output in a single JSON object with keys 'city' and 'population'. Output nothing outside the JSON."
    success:
      answer: "{\"city\": \"Paris\", \"population\": \"about 2.1 million\"}"
      why: "The whole response is one parseable JSON object with exactly the requested keys and no surrounding prose — the JSON verifier passes."
    failure:
      answer: "The capital of France is Paris. {\"city\": \"Paris\", \"population\": \"about 2.1 million\"}"
      why: "✓ correct facts, ✓ valid JSON embedded, ✗ the leading sentence sits outside the JSON — the 'wrap the entire output in JSON, nothing outside it' constraint is violated."
    checks: "Whether the model confines its whole response to a valid, parseable structured format."
    scoring: "Instruction-following accuracy — the verifier tries to parse the full response as JSON; extra text fails it."
  - task_type: "Title marker + required placeholders"
    input: "Write a short template email declining a meeting. Include a title wrapped in double angle brackets, like <<Title>>, and use at least two placeholders in square brackets such as [name] and [date]."
    success:
      answer: "<<Declining the Meeting>>\n\nHi [name],\n\nThank you for the invite, but I won't be able to join on [date]. Please share notes afterward and I'll follow up. Best regards, [your name]"
      why: "A title appears in double angle brackets and three bracketed placeholders are present — both the title-format and placeholder-count verifiers pass."
    failure:
      answer: "<<Declining the Meeting>>\n\nHi [name],\n\nThank you for the invite, but I won't be able to join the meeting this week. Please share notes afterward and I'll follow up. Best regards."
      why: "✓ well-formed title in double angle brackets, ✓ polite and usable, ✗ only one bracketed placeholder ([name]) appears — the 'at least two placeholders' constraint is violated."
    checks: "Whether the model satisfies two independent structural markers in one response."
    scoring: "Instruction-following accuracy — separate verifiers check the title delimiter and the placeholder count; both must pass."
tags: [instruction-following, formatting, deterministic]
updated: 2026-11-23
sources:
  - { url: "https://arxiv.org/abs/2311.07911", title: "Instruction-Following Evaluation for Large Language Models (Zhou et al., 2023)", trust: 0.95 }
  - { url: "https://ar5iv.labs.arxiv.org/html/2311.07911", title: "IFEval paper, full text (ar5iv)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/google/IFEval", title: "google/IFEval — dataset card (541 prompts, Apache 2.0)", trust: 0.9 }
  - { url: "https://github.com/google-research/google-research/tree/master/instruction_following_eval", title: "instruction_following_eval — official code (google-research)", trust: 0.9 }
related: [benchmark/ifbench]
---

## What It Tests
**Literal instruction adherence**: does the response satisfy explicit, machine-checkable constraints?

## How It Scores
Verification functions check each instruction. Four numbers:

- **Prompt-level strict** — every instruction in the prompt followed.
- **Instruction-level strict** — share of instructions followed.
- **Loose variants** — both again, after simple response transformations.

## Format
0-shot prompts; no judge model.

- **Size** — 541 prompts in one split, each with one or more of 25 instruction types.
- **Row fields** — `key`, `prompt`, `instruction_id_list` (the instructions to check) and `kwargs` (their arguments, such as a word count).
- **Licence** — Apache 2.0 on the Hugging Face dataset card.

## Limitations
- **Form, not substance.** A useless answer can pass every check.
- **Strict vs loose.** Loose reduces false negatives but adds false positives.
- **Finite coverage.** 25 instruction types.

## Example
From the public dataset on Hugging Face (key 1001, Apache 2.0): "I am planning a trip to Japan, and I would like thee to write an itinerary for my journey in a **Shakespearean style**." The rest of the prompt forbids commas.

- **Instruction checked** — `punctuation:no_comma`.
- **Verification** — code scans the response for any comma; one comma fails the prompt, however good the itinerary.
