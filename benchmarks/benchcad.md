---
id: benchmark/benchcad
type: benchmark
slug: benchcad
schema_version: 1.0
title: "BenchCAD"
name: BenchCAD (including BenchCAD 2.1)
category: Code Generation
description: "A benchmark of 17,900 execution-verified CadQuery programs across 106 industrial part families, testing CAD question answering, image-to-code generation and code editing."
what_is_it: "BenchCAD is a unified benchmark for industrial computer-aided design (CAD) reasoning. It contains 17,900 execution-verified CadQuery programs across 106 industrial part families, such as bevel gears, compression springs and twist drills. Models are evaluated through visual question answering, code question answering, image-to-code generation and instruction-guided code editing, which separates perception, parametric abstraction and executable program synthesis."
why_people_use_it: "Industrial CAD code generation needs more than recognising an outer shape: a model must understand 3D structure, infer engineering parameters and choose operations that reflect how a part is designed and made. The authors find that models often recover coarse geometry but miss fine structure and replace operations such as sweeps, lofts and twist-extrudes with simpler sketch-and-extrude patterns."
method: "Programs are verified by execution. The four tasks are visual question answering, code question answering, image-to-code generation and instruction-guided code editing. The paper evaluates over ten frontier multimodal models and open CAD-specialised baselines."
metric: "Task accuracy or similarity to the reference part. Anthropic's release describes BenchCAD 2.1 as measuring how closely a model's designs match reference geometry and circuits, scored 0 to 100 with partial credit."
format: "Visual and code inputs, CadQuery programs or answers as output, graded by execution."
limitations: "Fine-tuning and reinforcement learning improve in-distribution results, but the authors report limited generalisation to unseen part families. The paper describes the first release; a version 2.1 appears in Anthropic's model release, which also describes its 0-to-100 partial-credit scale."
citation: "https://arxiv.org/abs/2605.10865"
tags: [cad, engineering, code-generation, multimodal, hardware]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2605.10865", title: "BenchCAD: A Comprehensive, Industry-Standard Benchmark for Programmatic CAD (arXiv:2605.10865)", trust: 0.95 }
  - { url: "https://www.anthropic.com/claude-fable-and-mythos-5-2", title: "Introducing Claude Fable 5.2 and Claude Mythos 5.2 (Anthropic, BenchCAD 2.1)", trust: 0.8 }
---

## What It Tests
Whether a model understands **industrial CAD** well enough to write faithful parametric programs.

- **Perception** — reading the part from rendered views.
- **Parametric abstraction** — inferring engineering parameters.
- **Program synthesis** — producing CadQuery that executes.

## How It Scores
**Execution-verified** programs and tasks.

- **Four tasks** — visual QA, code QA, image-to-code and code editing.
- **Version 2.1** — Anthropic describes a 0–100 score with partial credit.

## Format
17,900 programs in 106 part families. Fine-tuned and reinforcement-learning-trained models are included in the paper's analysis.

## Limitations
- **Generalisation.** Training helps on seen families, less on unseen ones.
- **Versions.** The paper covers the first release; 2.1 is described by Anthropic.

## Example
The part families show the kind of item: a **bevel gear**, a **compression spring** or a **twist drill**. In image-to-code, the model sees rendered views and must write a CadQuery program that rebuilds the part; the paper reports that models tend to use a plain sketch-and-extrude where a part truly needs a sweep, loft or twist-extrude.
