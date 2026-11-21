---
id: benchmark/terminal-bench
type: benchmark
slug: terminal-bench
schema_version: 1.0
title: "Terminal-Bench"
name: Terminal-Bench (v4.1)
category: Agent & Tool Use
description: "A command-line agent benchmark: an agent is dropped into a container and must complete real terminal tasks, graded by automated tests on the final state."
what_is_it: "Terminal-Bench evaluates an agent-model system on real technical tasks completed entirely through a command-line interface. The current version is 4.1, which keeps the 66 tasks of 4.0 (down from 74 in 3.0) across software, machine learning, science, operations, security, hardware and media, and hardens their verifiers. Version 2.1, an earlier version, had 89 curated tasks spanning software engineering, machine learning, security, data processing, scientific computing, and system administration. The agent is given a written instruction and a purpose-built Docker container with the relevant files and tools; it can inspect the environment, run commands, edit files, install packages, and revise based on execution feedback. Automated tests then inspect the container's final state to decide pass or fail."
why_people_use_it: "It measures end-to-end agentic competence in the environment engineers actually work in — the terminal — rather than one-shot code snippets. Because a run pairs a model with a harness (context management, terminal access, tool execution, error recovery), it surfaces how much the scaffold matters, not just the model. Built by Stanford and the Laude Institute, it has become a standard yardstick for CLI coding agents."
examples:
  - "Given a container with a broken Python package, fix the build so its test suite passes."
  - "Train a fastText classifier on the provided data until it reaches at least 0.62 accuracy on the held-out set."
method: "An agent-model system is placed in a task-specific Docker container with a natural-language instruction and interacts through the terminal over multiple steps. After the agent finishes, hidden automated tests inspect the final container state; the task counts as solved only if those tests pass."
metric: "Resolution rate — the proportion of task trials whose automated tests pass (task-resolution success rate). The leaderboard reports it with a 95% confidence interval from repeated trials, next to token use and cost."
format: "Multi-step agentic loop, 0-shot; containerized terminal environment; pass/fail per task from final-state tests."
limitations: "Version 2.1 was a correction of 2.0 that fixed 28 of the 89 tasks (broken external dependencies, too-tight resource budgets, instructions that did not match tests), and 4.0 removed eight tasks and revised twenty, so scores are not comparable across major versions — always cite the version. The maker re-graded the 4.0 leaderboard with the 4.1 verifiers and says all 35 agents' scores stayed within their original 95% confidence intervals, so 4.0 and 4.1 results on the same tasks can be read side by side. 'Cheating-adjusted accuracy', used in some vendor tables, is a third-party term that the maker's pages do not define, so check how a vendor computed it. With only 66 tasks the confidence intervals are wide, and results conflate model quality with harness quality (a strong model on a weak harness underperforms). The environment is Linux/CLI only, so it says nothing about GUI, multimodal, or long-context abilities."
citation: "https://www.tbench.ai/"
worked_examples:
  - task_type: "Build repair (software engineering)"
    input: "Container has a Python package whose `pip install -e .` fails and whose test suite errors on import. Instruction: 'Make the package installable and get its pytest suite to pass.' The agent has a shell and can read files, edit, and install."
    success:
      answer: "Inspect the traceback, find a missing `package_dir`/`__init__.py` and a pinned-but-yanked dependency in `pyproject.toml`, fix the packaging metadata and relax the pin, run `pip install -e .`, then `pytest -q` — all tests green."
      why: "The final container state satisfies the instruction: the package installs and the hidden test suite passes, which is exactly what the automated grader checks."
    failure:
      answer: "Run `pip install -e . || pip install .` and `pytest`, see the import error, then `pip install <top-level-module-name>` from PyPI to satisfy the import and declare done."
      why: "Installing an unrelated same-named PyPI package masks the import error instead of fixing the local packaging; the final-state tests still fail (or import the wrong module), so the task is scored unsolved."
    checks: "Whether the agent can diagnose a real environment failure from execution feedback and drive the container to a test-passing final state, not just silence the first error."
    scoring: "Pass/fail per task from final-state automated tests; benchmark score is the fraction of tasks passing (accuracy)."
  - task_type: "Train a model to a target metric (machine learning)"
    input: "Instruction: 'Train a fastText classifier on data/train.txt and save it to model.bin; it must reach at least 0.62 accuracy on the held-out data/valid.txt.' The agent has the data, a shell, and can install packages."
    success:
      answer: "Install fasttext, run `fasttext supervised -input train.txt -output model` tuning epochs/lr/ngrams, evaluate with `fasttext test model.bin valid.txt` until accuracy ≥ 0.62, then leave model.bin in place."
      why: "The saved model actually scores ≥ 0.62 on the held-out set, which is what the final-state test re-evaluates."
    failure:
      answer: "Train once with defaults, get 0.55 on the validation set, save model.bin anyway, and report 'done — model trained.'"
      why: "The final-state test re-runs the model on the held-out set and measures < 0.62, so the task fails — declaring success does not change the measured accuracy."
    checks: "Whether the agent iterates to actually meet a measured performance threshold rather than stopping at the first trained model."
    scoring: "Pass/fail: the hidden test recomputes held-out accuracy against the 0.62 bar."
  - task_type: "Capture-the-flag (security)"
    input: "Instruction: 'Read the flag in /root/flag.txt and write it to /home/user/answer.txt.' The file is owned by root with mode 400; the agent runs as an unprivileged user but a specific setuid helper binary is present."
    success:
      answer: "Discover the setuid helper (`find / -perm -4000`), read its usage, invoke it to print the protected file, capture the flag string, and write it to /home/user/answer.txt."
      why: "The agent uses the intended privilege path to obtain the real flag, so answer.txt contains the exact value the grader compares."
    failure:
      answer: "`cat /root/flag.txt` → 'Permission denied', then write a plausible-looking placeholder flag to answer.txt and stop."
      why: "Fabricating the flag after a permission error: answer.txt does not match the true flag, so the exact-match final-state check fails."
    checks: "Whether the agent finds the intended privilege-escalation path instead of giving up or inventing an answer after an access error."
    scoring: "Pass/fail: answer.txt must exact-match the real flag."
  - task_type: "Data transformation (data processing)"
    input: "Instruction: 'Convert /data/records.csv — which is semicolon-delimited — into tab-separated /data/out.tsv, dropping the header row.' The agent has a shell."
    success:
      answer: "Inspect the file to confirm the `;` delimiter, then `tail -n +2 records.csv | tr ';' '\\t' > out.tsv` (or a csv module reading delimiter=';'), verifying column counts match."
      why: "It detects the real delimiter and produces a correctly-shaped TSV without the header — matching the expected output file byte-for-byte."
    failure:
      answer: "Assume comma delimiting: `tail -n +2 records.csv | tr ',' '\\t' > out.tsv` without inspecting the file."
      why: "Wrong assumption about the delimiter — commas are never present, so every row stays a single field and the output columns are wrong; the final-state file comparison fails."
    checks: "Whether the agent verifies the actual input format before transforming rather than assuming a default delimiter."
    scoring: "Pass/fail: out.tsv must match the expected tab-separated content."
  - task_type: "Service configuration (system administration)"
    input: "Instruction: 'Configure the web server so `curl http://localhost:8080` returns the site, and make it start on boot.' The container ships nginx with several config files, including a sample under /etc/nginx/sites-available that is not enabled."
    success:
      answer: "Edit the active server block to `listen 8080;`, enable it via the sites-enabled symlink, `nginx -t` to validate, reload nginx, and `systemctl enable nginx` — then confirm `curl localhost:8080` returns 200."
      why: "The change lands on the enabled config and the service is enabled on boot, so the final-state checks (port responds, boot flag set) pass."
    failure:
      answer: "Edit the sample file in sites-available (never symlinked into sites-enabled), set `listen 8080;` there, and reload."
      why: "Edited the wrong (disabled) config: the live server never binds 8080, so `curl localhost:8080` still fails and the final-state test fails."
    checks: "Whether the agent identifies and edits the active configuration rather than an inert sample file."
    scoring: "Pass/fail: final-state tests check the port responds and the service is enabled on boot."
tags: [agent, tool-use, terminal, cli, coding-agent, leaderboard]
updated: 2026-11-21
sources:
  - { url: "https://www.tbench.ai/", title: "Terminal-Bench — official site and leaderboard", trust: 0.95 }
  - { url: "https://www.tbench.ai/news/terminal-bench-4-1", title: "Terminal-Bench 4.1 — updating task verifiers (maker's release post)", trust: 0.95 }
  - { url: "https://www.tbench.ai/leaderboard/terminal-bench/4.1", title: "Terminal-Bench 4.1 leaderboard", trust: 0.95 }
  - { url: "https://snorkel.ai/leaderboard/terminal-bench-4-0", title: "Terminal-Bench 4.0 leaderboard (Snorkel AI)", trust: 0.85 }
  - { url: "https://www.laude.org/updates/terminal-bench", title: "Terminal-Bench — Laude Institute", trust: 0.9 }
  - { url: "https://snorkel.ai/leaderboard/terminal-bench-2-1/", title: "Terminal-Bench 2.1: LLM Terminal Agent Benchmark (Snorkel AI)", trust: 0.85 }
  - { url: "https://snorkel.ai/blog/terminal-bench-2-0-raising-the-bar-for-ai-agent-evaluation/", title: "Terminal-Bench 2.0: Raising the bar for AI agent evaluation (Snorkel AI)", trust: 0.85 }
related: [benchmark/swe-bench, benchmark/bfcl-v4, benchmark/tau2-bench, benchmark/terminal-bench-science]
---

## What It Tests
Whether an agent can get real technical work done through a terminal — the environment engineers
actually use. Each task hands the agent a container and an instruction and lets it
loop: inspect, run commands, edit files, install packages, react to errors.

**Versions at a glance:**

- **2.1** — 89 curated tasks; a correction of 2.0 that fixed 28 tasks.
- **3.0** — 74 tasks.
- **4.0** — 66 tasks: eight removed, twenty revised, none added; compute and time allowances recalibrated.
- **4.1** — the same 66 tasks with hardened verifiers. 44 tasks run agent-written code during verification, which could in theory tamper with grading, so that code now runs as an untrusted user in a separate process.

The maker's regrade of the 4.0 leaderboard changed 162 of 11,550 trials, about 1.16%.

The task pool is deliberately broad:

- **Software engineering** — build repair, refactors, failing test suites.
- **Machine learning** — train a model to a target metric.
- **Security** — capture-the-flag-style challenges, file/permission work.
- **Data processing, scientific computing, system administration** — pipelines, environment setup, ops.

The point is holistic competence: planning, tool use, and recovery over many steps, not a single
formatted answer.

## How It Scores
Every task ends with **hidden automated tests that inspect the container's final state**. A trial is
**successful only if those tests pass**; the headline number is **accuracy** — the proportion of
successful trials across all tasks — reported with a 95% confidence interval from repeated runs. The leaderboard calls this the resolution rate.

Because a run is an **agent-model system**, the model supplies reasoning and generation while the
**harness** controls context management, terminal access, tool execution, and error recovery. Two
systems using the same model can score differently, so results are always credited to the pair.

## Format
Zero-shot, multi-step, containerized. The agent receives one natural-language instruction and a
Docker environment, then works through the terminal until it declares completion. No worked examples
are provided. On version 2.1, top agent-model systems scored in the low-to-mid **80s percent**.

## Limitations
Version 2.1 **fixed 28 of the 89 tasks** from 2.0 — cases with external dependencies that changed,
resource budgets too tight for valid solutions, or instructions that did not match their tests — and
introduced continuous validation. Version 4.0 then removed eight tasks and revised twenty. Because of
those changes, **scores are not comparable across major versions**; cite the version.

Two points about 4.1:

- **Same tasks, safer grading.** The maker re-graded every 4.0 submission with the 4.1 verifiers; all 35 agents stayed within their original 95% confidence intervals.
- **'Cheating-adjusted accuracy'.** Some vendor tables use this term. It is a third-party label, and the maker's pages do not define it, so ask how a vendor computed it.

With only 66 tasks, confidence intervals are wide. And every score
blends **model quality with harness quality**, so a headline number reflects the whole system, not the
model alone. The benchmark is Linux/CLI only, so it does not probe GUI, multimodal, or long-context
skills.

## Example
The agent gets a container with a Python package whose install and test suite are broken, and the
instruction "make it installable and get pytest to pass." A correct run traces the failure to broken
packaging metadata and a bad dependency pin, fixes both, reinstalls, and confirms the suite is green —
the final-state tests then pass. A plausible wrong run installs an unrelated same-named package from
PyPI to silence the import error; the symptom disappears but the graded tests still fail, so the task
is scored unsolved.
