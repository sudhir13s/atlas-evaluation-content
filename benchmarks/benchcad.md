---
id: benchmark/benchcad
type: benchmark
slug: benchcad
schema_version: 3.1
title: "BenchCAD"
name: BenchCAD (including BenchCAD 2.1)
category: Code Generation
description: "A benchmark of 17,900 execution-verified CadQuery programs across 106 industrial part families, testing CAD question answering, image-to-code generation and code editing."
what_is_it: "BenchCAD (Haozhe Zhang and colleagues, May 2026) is a unified benchmark for industrial computer-aided design (CAD) reasoning. It contains 17,900 execution-verified CadQuery programs across 106 industrial part families, such as bevel gears, compression springs and twist drills. Models are evaluated through visual question answering, code question answering, image-to-code generation and instruction-guided code editing, which separates perception, parametric abstraction and executable program synthesis. CadQuery is a Python library that describes 3D parts as code."
why_people_use_it: "Industrial CAD code generation needs more than recognising an outer shape: a model must understand 3D structure, infer engineering parameters and choose operations that reflect how a part is designed and made. The authors find that models often recover coarse geometry but miss fine structure and replace operations such as sweeps, lofts and twist-extrudes with simpler sketch-and-extrude patterns."
examples:
  - "How many teeth does the visible gear have? (a question template for involute gears, from the paper's Appendix J)"
method: "Domain experts wrote a parameterised builder for each part family; every generated CadQuery program was run in a sandbox, failures were filtered out and survivors got expert visual sign-off. Question answers are derived from the generating parameters. Image-to-code outputs are executed and compared with the reference part; edits are compared with the target part. The paper evaluates more than ten frontier multimodal models and open CAD-specialised baselines."
metric: "Per task. Question answering: accuracy, with a 5% tolerance for ratios and exact match for integers. Image-to-code: execution rate, voxel IoU (overlap of the two solids / their union), Chamfer and Hausdorff distances, Feature-F1, essential-operation recall and a composite total score. Code editing: normalised accuracy, the share of the original-to-target IoU gap the edit closes, clipped to 0-1. Anthropic describes BenchCAD 2.1 as scored 0 to 100 with partial credit."
format: "Rendered views or code in; numeric answers, CadQuery programs or edited programs out, graded by execution. BenchCAD-QA has 2,400 numeric questions, each asked with image and with code conditioning (4,800 records); BenchCAD-Edit has 748 edit pairs."
limitations: "Fine-tuning and reinforcement learning improve in-distribution results, but the authors report limited generalisation to unseen part families. The parts are procedurally generated from expert-written builders, so they cover the 106 families rather than arbitrary designs. The paper describes the first release; a version 2.1 appears in Anthropic's model release, which describes it as measuring how closely designs match reference geometry and circuits, and does not say who made it. The data is public, so it can appear in training data."
citation: "https://arxiv.org/abs/2605.10865"
openness: open
openness_note: "The parts, questions and edit pairs are on Hugging Face under CC-BY-4.0 and the evaluation code is on GitHub under MIT, so anyone can rerun the first release; version 2.1 is described only in a vendor release."
tags: [cad, engineering, code-generation, multimodal, hardware]
updated: 2026-11-23
sources:
  - { url: "https://arxiv.org/abs/2605.10865", title: "BenchCAD: A Comprehensive, Industry-Standard Benchmark for Programmatic CAD (arXiv:2605.10865)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2605.10865", title: "BenchCAD paper, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/BenchCAD/BenchCAD", title: "BenchCAD/BenchCAD — Hugging Face dataset (CC-BY-4.0)", trust: 0.9 }
  - { url: "https://github.com/BenchCAD/BenchCAD-main", title: "BenchCAD/BenchCAD-main — evaluation code (MIT)", trust: 0.9 }
  - { url: "https://www.anthropic.com/claude-fable-and-mythos-5-2", title: "Introducing Claude Fable 5.2 and Claude Mythos 5.2 (Anthropic, BenchCAD 2.1)", trust: 0.8 }
---

## What It Tests
Whether a model understands **industrial CAD** well enough to write faithful parametric programs.

- **Perception** — reading the part from rendered views.
- **Parametric abstraction** — inferring engineering parameters such as a gear's tooth count.
- **Program synthesis** — producing CadQuery that executes and rebuilds the part.

**Who built it:** Haozhe Zhang, Kaichen Liu, Miaomiao Chen, Lei Li, Shaojie Yang, Cheng Peng and Hanjie Chen (arXiv, May 2026). Project page: [benchcad.github.io](https://benchcad.github.io/BenchCAD_webpage/).

## How It Scores
Every reference part is **execution-verified**, and each task has its own metric.

| Task | Main metric |
|---|---|
| Question answering (image or code) | Accuracy; 5% tolerance on ratios, exact match on integers |
| Image-to-code | Execution rate, voxel IoU, Chamfer and Hausdorff distance, Feature-F1, essential-operation recall, composite score |
| Code editing | Normalised accuracy: share of the IoU gap to the target that the edit closes |

- **IoU** (intersection over union) — the volume both solids share / the volume either one covers.
- **Version 2.1** — Anthropic describes a 0–100 score with partial credit.

## Format
17,900 programs in 106 part families.

- **BenchCAD-QA** — 2,400 numeric questions, each asked once with an image and once with code (4,800 records).
- **BenchCAD-Edit** — 748 instruction-guided edit pairs.
- **Training study** — the paper fine-tunes a small model with 10 mechanical families held out to test generalisation.

## Limitations
- **Generalisation.** Training helps on seen families, less on unseen ones.
- **Generated parts.** Items come from expert-written builders for 106 families.
- **Versions.** The paper covers the first release; 2.1 is described only by Anthropic.
- **Public items.** The data can leak into training sets.

## Example
From the paper's Appendix J, a question template for an **involute gear**: "how many teeth does the visible gear have?"

- **With an image** — the model sees rendered views of the gear and answers a number.
- **With code** — the model reads the CadQuery program instead.
- **Scoring** — integer answers must match exactly; the key comes from the parameters that generated the part.

In image-to-code, the paper reports that models tend to use a plain sketch-and-extrude where a part truly needs a sweep, loft or twist-extrude.
