---
name: research-program
description: Use when turning a research question into paper, repo, dataset, docs discovery, baseline-first planning, controlled experiments, or one-shot agent-operated research loops across Claude Code, Codex, or Cursor
---

# Research Program

## Overview

Turn open-ended research into a bounded, reusable program. Borrow `ml-intern` for discovery and trace discipline, and `autoresearch` for baseline-first, metric-driven, keep/discard experiments.

Default mode is **one-shot**: run at most one baseline or one experiment, record it, then stop and report. Continuous or overnight loops require explicit user instruction.

## When to Use

Use for:
- literature, paper, repo, dataset, model, benchmark, or docs research
- turning a topic into an experiment plan
- writing a `program.md` that Claude Code, Codex CLI, or Cursor Agent can follow
- running controlled baseline/experiment cycles
- adapting `ml-intern` or `autoresearch` ideas without requiring their model API keys

Do not use for:
- normal bug fixes with no research question
- unrestricted autonomous loops
- experiments without a metric or stop condition

## Core Rules

| Rule | Why |
|---|---|
| Separate sourced facts, user context, inference, and recommendation | Prevents fake certainty |
| Define metric before experiments | Prevents goal drift |
| Run baseline before changes | Makes results comparable |
| Edit only allowed files | Keeps research harness stable |
| Record every run in `results.tsv` | Avoids chat-only memory |
| Stop after one run by default | Prevents runaway loops |

## Workflow

1. **Frame the question**
   - Goal, hypothesis, target metric, constraints, compute budget, stop condition.
2. **Gather evidence**
   - Papers, repos, datasets, docs, model cards, benchmark references.
   - Label each item as sourced fact, user-provided context, or inference.
3. **Choose baseline**
   - Pick the simplest reproducible baseline that matches the metric and compute.
4. **Write or update `program.md`**
   - Define allowed files, forbidden files, commands, metric, logging, and stop rules.
5. **Run baseline or one experiment**
   - If no baseline exists, run baseline only.
   - If baseline exists, run exactly one scoped experiment.
6. **Record result**
   - Append `results.tsv`, preserve logs, summarize decision.
7. **Report and stop**
   - Recommend next experiment, but do not run it unless the user asks.

## Cross-Assistant Use

### Claude Code

Invoke this skill, then operate with local tools. Use task tracking for multi-step work. Prefer existing repo commands. Do not claim success until the proving command ran.

### Codex CLI

Tell Codex:

```text
Read program.md and follow it exactly. Run the baseline if none exists. Otherwise run one experiment only, record results.tsv, then stop.
```

### Cursor Agent

Open the repo and tell Cursor:

```text
Read program.md and follow it exactly. Run the baseline if none exists. Otherwise run one experiment only, record results.tsv, then stop.
```

## Suggested Files

```text
research/
  notes.md
  papers.md
  repos.md
  datasets.md
  experiment_plan.md
  program.md
  results.tsv
  runs/
```

Use this inside an existing repo when possible:

```text
program.md
results.tsv
runs/<tag>/run.log
```

## `program.md` Template

````markdown
# Research Program

## Goal
[What question are we answering?]

## Success Metric
Primary metric: [name, direction, command that computes it]
Secondary metrics: [runtime, cost, memory, quality]

## Constraints
Compute budget:
Time budget per run:
Allowed dependencies:
Stop condition:

## Evidence Sources
Papers:
Repos:
Datasets:
Docs:

## Allowed to Modify
- [file or directory]

## Forbidden to Modify
- [evaluation script]
- [data split]
- [metric definition]
- [lockfiles unless dependency changes are explicitly approved]

## Baseline Command
```bash
[command]
```

## Experiment Command
```bash
[command]
```

## Result Logging
Append to `results.tsv`:

```text
run_id\tcommit\tmetric\tstatus\tdescription\tlog_path
```

Status values:
- `baseline`
- `keep`
- `discard`
- `crash`
- `blocked`

## Operating Loop
1. Inspect git state and existing `results.tsv`.
2. If no baseline exists, run baseline only, record it, and stop.
3. If baseline exists, propose one hypothesis.
4. Change only allowed files.
5. Run the experiment command.
6. Append `results.tsv`.
7. Keep the change only if metric improves or user-defined criteria says keep.
8. Stop and report next recommendation.

## Continuous Mode
Do not continue beyond one run unless the user explicitly says: continuous, overnight, loop, or keep going autonomously.

````

## `results.tsv` Format

```text
run_id	commit	metric	status	description	log_path
baseline	abc1234	0.8123	baseline	initial baseline	runs/baseline/run.log
exp001	def5678	0.8051	keep	lower learning rate	runs/exp001/run.log
exp002	0000000	0.0000	crash	larger batch OOM	runs/exp002/run.log
```

## Common Mistakes

| Mistake | Fix |
|---|---|
| Starting experiments before baseline | Run baseline first |
| Changing metric or eval script mid-run | Mark blocked and ask user |
| Letting agent run many experiments by default | Stop after one run |
| Mixing facts and guesses | Label evidence boundaries |
| Recording only in chat | Write `results.tsv` and logs |
| Copying `autoresearch` infinite loop literally | Require explicit continuous-mode instruction |
| Trying to spend Claude Code or ChatGPT subscription quota through `ml-intern` | Run the workflow inside that assistant instead |

## Report Format

```text
QUESTION
- [research question]

EVIDENCE
- sourced facts:
- user-provided context:

BASELINE / EXPERIMENT
- command:
- metric:
- log:

RESULT
- status:
- decision:

NEXT
- recommended next experiment:
- whether user approval is needed:
```
