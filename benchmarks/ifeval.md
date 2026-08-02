---
id: benchmark/ifeval
type: benchmark
slug: ifeval
schema_version: 1.0
title: "IFEval"
name: IFEval
category: Instruction Following & Chat
description: "Instruction-following measured by verifiable, programmatically checkable constraints."
what_is_it: "A benchmark of ~540 prompts carrying 'verifiable instructions' — constraints a script can check without human or model judgment, such as 'respond in exactly 3 paragraphs', 'do not use any commas', 'include the keyword X at least twice', or 'answer in JSON'. It isolates whether a model actually obeys formatting and constraint instructions."
why_people_use_it: "It removes judge-model subjectivity: correctness is decided by code, not opinion. That makes it a clean, cheap, reproducible measure of instruction adherence, and it anchors the HF Open LLM Leaderboard v2."
examples:
  - "Write a summary of the water cycle in exactly two paragraphs, and do not use the word 'water'. (checked programmatically for paragraph count and forbidden word)"
method: "0-shot; each response is checked by deterministic verifier functions. Reported at strict/loose granularity and at prompt vs instruction level."
metric: "Instruction-following accuracy (% of constraints satisfied); prompt-level and instruction-level, each in strict and loose variants."
format: "0-shot, programmatic verification."
saturation: medium
limitations: "Only measures verifiable-constraint following, not answer quality — a model can satisfy every format rule while giving a useless answer. Coverage of instruction types is finite. 'Loose' vs 'strict' scoring (which tolerates minor formatting like markdown wrappers) can differ by several points."
citation: "https://arxiv.org/abs/2311.07911"
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
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2311.07911", title: "Instruction-Following Evaluation for Large Language Models (Zhou et al., 2023)", trust: 0.95 }
related: [benchmark/mmlu-pro]
---

## What It Tests
Literal instruction adherence: does the model honor explicit, machine-checkable constraints on its
output? It deliberately avoids anything requiring judgment, so the signal is pure compliance.

## How It Scores
Deterministic verifier functions check each constraint. Results are reported four ways:
prompt-level (all constraints in a prompt satisfied) vs instruction-level (each constraint counted),
each in **strict** (exact) and **loose** (tolerant of trivial wrappers like surrounding markdown)
variants. The four numbers together give a fuller picture than any one.

## Format
0-shot. No few-shot examples and no judge model — just the prompt, the response, and the checkers.

## Limitations
It measures **form, not substance**: a response can pass every check and still be unhelpful or wrong.
The instruction taxonomy is finite, so it does not cover every kind of directive. The strict/loose
gap is a real reporting subtlety — always state which you quote.

## Example
"Write a summary of the water cycle in exactly two paragraphs, and do not use the word 'water'." The
verifier counts paragraphs (must equal 2) and scans for the forbidden word. A beautifully written
three-paragraph summary fails on the paragraph-count constraint regardless of its quality — which is
the point.
