---
id: benchmark/terminal-bench
type: benchmark
slug: terminal-bench
schema_version: 4.0
title: Terminal-Bench
name: Terminal-Bench (v4.1)
category: Agent & Tool Use
description: 'A command-line agent benchmark: an agent is dropped into a container and must complete real terminal tasks, graded by automated tests on the final state.'
what_is_it: Terminal-Bench evaluates an agent-model system on real technical tasks completed entirely through a command-line interface. It is hosted by Stanford, Harbor and the Laude Institute, with project leadership from Ryan Marten, Alex Shaw, Andy Konwinski and Ludwig Schmidt. The first release appeared in May 2025; version 2.0 (November 2025) had 89 tasks, 3.0 (July 2026) started a new set of 74 tasks, 4.0 (August 2026) removed 8 of them, and 4.1 (November 2026) keeps the resulting 66 tasks and hardens their verifiers. The agent gets a written instruction and a purpose-built Docker container, and automated tests inspect the container's final state to decide pass or fail.
why_people_use_it: It measures end-to-end agentic work in the terminal rather than one-shot code snippets. Because a run pairs a model with a harness (context management, terminal access, tool execution, error recovery), it shows how much the scaffold matters, not just the model. The maker runs it as a continuous benchmark with tagged releases, and the tasks and verifiers are public, so anyone can rerun them.
examples:
- 'From the public 4.x task data-anonymization: build a command-line tool that anonymizes a set of related CSV files under a per-column policy, keeping file structure intact, giving the same entity the same token across files, staying deterministic for a given seed and within a memory cap.'
method: An agent-model system is placed in a task-specific Docker container with a natural-language instruction and interacts through the terminal over multiple steps. After the agent finishes, automated tests (the verifier) inspect the final container state; the task counts as solved only if those tests pass. Since 4.0 every task has a flat agent timeout of 8 hours.
metric: 'Resolution rate: resolved trials / all task trials, in percent. The leaderboard shows it with a 95% confidence interval from repeated trials.'
format: Multi-step agentic loop, 0-shot; containerized terminal environment; pass/fail per task from final-state tests.
limitations: 'Task sets change between major versions: 2.1 fixed 28 of the 89 tasks of 2.0, 3.0 is a new 74-task set, and 4.0 removed 8 tasks and revised 19, so scores are not comparable across major versions and the version must be cited. The maker re-graded the 4.0 leaderboard with the 4.1 verifiers and says all 35 agents'' updated scores fall within their original 95% confidence intervals. ''Cheating-adjusted accuracy'', used in some vendor tables, is not defined on the maker''s pages, so check how a vendor computed it. With 66 tasks the confidence intervals are wide, and each score blends model quality with harness quality. The environment is Linux command line only.'
citation: https://www.tbench.ai/
openness: open
openness_note: The tasks, environments and verifiers are public in the harbor-framework/terminal-bench repository under the Apache-2.0 licence and on the Harbor hub, so anyone can rerun them; the maker asks that the data never appear in training corpora.
worked_examples:
- task_type: Build repair (software engineering)
  input: 'Container has a Python package whose `pip install -e .` fails and whose test suite errors on import. Instruction: ''Make the package installable and get its pytest suite to pass.'' The agent has a shell and can read files, edit, and install.'
  success:
    answer: Inspect the traceback, find a missing `package_dir`/`__init__.py` and a pinned-but-yanked dependency in `pyproject.toml`, fix the packaging metadata and relax the pin, run `pip install -e .`, then `pytest -q` — all tests green.
    why: 'The final container state satisfies the instruction: the package installs and the hidden test suite passes, which is exactly what the automated grader checks.'
  failure:
    answer: Run `pip install -e . || pip install .` and `pytest`, see the import error, then `pip install <top-level-module-name>` from PyPI to satisfy the import and declare done.
    why: Installing an unrelated same-named PyPI package masks the import error instead of fixing the local packaging; the final-state tests still fail (or import the wrong module), so the task is scored unsolved.
  checks: Whether the agent can diagnose a real environment failure from execution feedback and drive the container to a test-passing final state, not just silence the first error.
  scoring: Pass/fail per task from final-state automated tests; benchmark score is the fraction of tasks passing (accuracy).
- task_type: Train a model to a target metric (machine learning)
  input: 'Instruction: ''Train a fastText classifier on data/train.txt and save it to model.bin; it must reach at least 0.62 accuracy on the held-out data/valid.txt.'' The agent has the data, a shell, and can install packages.'
  success:
    answer: Install fasttext, run `fasttext supervised -input train.txt -output model` tuning epochs/lr/ngrams, evaluate with `fasttext test model.bin valid.txt` until accuracy ≥ 0.62, then leave model.bin in place.
    why: The saved model actually scores ≥ 0.62 on the held-out set, which is what the final-state test re-evaluates.
  failure:
    answer: Train once with defaults, get 0.55 on the validation set, save model.bin anyway, and report 'done — model trained.'
    why: The final-state test re-runs the model on the held-out set and measures < 0.62, so the task fails — declaring success does not change the measured accuracy.
  checks: Whether the agent iterates to actually meet a measured performance threshold rather than stopping at the first trained model.
  scoring: 'Pass/fail: the hidden test recomputes held-out accuracy against the 0.62 bar.'
- task_type: Capture-the-flag (security)
  input: 'Instruction: ''Read the flag in /root/flag.txt and write it to /home/user/answer.txt.'' The file is owned by root with mode 400; the agent runs as an unprivileged user but a specific setuid helper binary is present.'
  success:
    answer: Discover the setuid helper (`find / -perm -4000`), read its usage, invoke it to print the protected file, capture the flag string, and write it to /home/user/answer.txt.
    why: The agent uses the intended privilege path to obtain the real flag, so answer.txt contains the exact value the grader compares.
  failure:
    answer: '`cat /root/flag.txt` → ''Permission denied'', then write a plausible-looking placeholder flag to answer.txt and stop.'
    why: 'Fabricating the flag after a permission error: answer.txt does not match the true flag, so the exact-match final-state check fails.'
  checks: Whether the agent finds the intended privilege-escalation path instead of giving up or inventing an answer after an access error.
  scoring: 'Pass/fail: answer.txt must exact-match the real flag.'
- task_type: Data transformation (data processing)
  input: 'Instruction: ''Convert /data/records.csv — which is semicolon-delimited — into tab-separated /data/out.tsv, dropping the header row.'' The agent has a shell.'
  success:
    answer: Inspect the file to confirm the `;` delimiter, then `tail -n +2 records.csv | tr ';' '\t' > out.tsv` (or a csv module reading delimiter=';'), verifying column counts match.
    why: It detects the real delimiter and produces a correctly-shaped TSV without the header — matching the expected output file byte-for-byte.
  failure:
    answer: 'Assume comma delimiting: `tail -n +2 records.csv | tr '','' ''\t'' > out.tsv` without inspecting the file.'
    why: Wrong assumption about the delimiter — commas are never present, so every row stays a single field and the output columns are wrong; the final-state file comparison fails.
  checks: Whether the agent verifies the actual input format before transforming rather than assuming a default delimiter.
  scoring: 'Pass/fail: out.tsv must match the expected tab-separated content.'
- task_type: Service configuration (system administration)
  input: 'Instruction: ''Configure the web server so `curl http://localhost:8080` returns the site, and make it start on boot.'' The container ships nginx with several config files, including a sample under /etc/nginx/sites-available that is not enabled.'
  success:
    answer: Edit the active server block to `listen 8080;`, enable it via the sites-enabled symlink, `nginx -t` to validate, reload nginx, and `systemctl enable nginx` — then confirm `curl localhost:8080` returns 200.
    why: The change lands on the enabled config and the service is enabled on boot, so the final-state checks (port responds, boot flag set) pass.
  failure:
    answer: Edit the sample file in sites-available (never symlinked into sites-enabled), set `listen 8080;` there, and reload.
    why: 'Edited the wrong (disabled) config: the live server never binds 8080, so `curl localhost:8080` still fails and the final-state test fails.'
  checks: Whether the agent identifies and edits the active configuration rather than an inert sample file.
  scoring: 'Pass/fail: final-state tests check the port responds and the service is enabled on boot.'
tags:
- agent
- tool-use
- terminal
- cli
- coding-agent
- leaderboard
updated: '2026-11-23'
sources:
- url: https://www.tbench.ai/
  title: Terminal-Bench — official site and leaderboard
  trust: 0.95
- url: https://www.tbench.ai/news
  title: Terminal-Bench news — release dates of every version
  trust: 0.95
- url: https://www.tbench.ai/news/terminal-bench-4-1
  title: Terminal-Bench 4.1 — updating task verifiers (maker's release post)
  trust: 0.95
- url: https://www.tbench.ai/news/terminal-bench-4-0
  title: Terminal-Bench 4.0 release post (8 removed, 19 revised, 8-hour timeout)
  trust: 0.95
- url: https://www.tbench.ai/news/terminal-bench-3-0
  title: Terminal-Bench 3.0 release post (74 tasks across 7 domains)
  trust: 0.95
- url: https://www.tbench.ai/news/terminal-bench-2-1
  title: Terminal-Bench 2.1 release post (89 tasks in 2.0, 28 fixed)
  trust: 0.95
- url: https://www.tbench.ai/leaderboard/terminal-bench/4.1
  title: Terminal-Bench 4.1 leaderboard
  trust: 0.95
- url: https://github.com/harbor-framework/terminal-bench
  title: harbor-framework/terminal-bench — official repository (Apache-2.0)
  trust: 0.9
- url: https://github.com/harbor-framework/terminal-bench/releases
  title: Terminal-Bench releases (v3.0.0 74 tasks, v4.0.0 removals, v4.1.0 no tasks added or removed)
  trust: 0.9
- url: https://raw.githubusercontent.com/harbor-framework/terminal-bench/main/tasks/data-anonymization/instruction.md
  title: Terminal-Bench task data-anonymization — instruction
  trust: 0.9
related:
- benchmark/swe-bench
- benchmark/bfcl-v4
- benchmark/tau2-bench
- benchmark/terminal-bench-science
---

## What It Tests

Whether an agent can get real technical work done through a terminal — the environment engineers
actually use. Each task hands the agent a container and an instruction and lets it
loop: inspect, run commands, edit files, install packages, react to errors.

**Versions at a glance** (from the maker's release posts and GitHub releases):

| Version | Released | Tasks | What changed |
|---|---|---|---|
| 2.0 | Nov 2025 | 89 | Harder set, launched with the Harbor framework |
| 2.1 | May 2026 | 89 | 28 tasks fixed (changed external dependencies, too-small resource budgets, misspecified instructions) |
| 3.0 | Jul 2026 | 74 | New set from 100+ contributors and reviewers, across 7 domains |
| 4.0 | Aug 2026 | 66 | 8 tasks removed, 19 revised, CPU and memory calibrated, flat 8-hour timeout |
| 4.1 | Nov 2026 | 66 | No tasks added or removed; verifiers hardened |

**Why 4.1 exists:** 44 tasks in 4.0 run agent-written code during verification, which could in theory tamper with grading. The maker's regrade of the 4.0 leaderboard changed **162 of 11,550 trials**, about 1.16%.

Task names in the public repository show the spread: CAD models, finance calculations, security (a UEFI bootkit task), GPU training speed-runs and database internals.

## How It Scores

Every task ends with **automated tests that inspect the container's final state**. A trial **passes only if those tests pass**.

- **Resolution rate** = resolved trials / all trials, in percent.
- **Confidence interval** — the leaderboard shows a 95% interval from repeated trials.
- **Credited to a pair** — the model supplies reasoning; the **harness** controls context, terminal access, tool execution and error recovery. Two systems with the same model can score differently.

## Format

Zero-shot, multi-step, containerized. The agent receives **one natural-language instruction** and a Docker environment, then works through the terminal until it declares completion.

- **Data and code** — tasks, environments and verifiers are public in `harbor-framework/terminal-bench` (**Apache-2.0**) and on the Harbor hub.
- **Canary** — the site asks that benchmark data never appear in training corpora.

## Limitations

- **Version-bound.** Task sets change between major versions; cite the version with any score.
- **4.0 versus 4.1.** The maker says all 35 re-graded agents stayed within their original 95% confidence intervals.
- **Undefined vendor metrics.** 'Cheating-adjusted accuracy' is not defined on the maker's pages.
- **Small set.** With 66 tasks, confidence intervals are wide.
- **System, not model.** Every score blends model quality with harness quality.
- **Terminal only.** Linux command line; no graphical interfaces.

## Example

From the public task `data-anonymization` (paraphrased from its instruction file; the maker asks that task data stay out of training corpora, so it is not quoted):

- **Build** a command-line tool at `/app/anon.py` that anonymizes a folder of related CSV files using per-column rules in a policy file.
- **Keep** filenames, headers, column order, row order and row counts unchanged.
- **Tokenize consistently** — the same real entity must get the same token in every file, including linked records.
- **Stay deterministic** for a given `--seed`, and within a `--max-memory` cap.

The verifier then runs the tool and checks the output files; the run passes only if every check holds.
