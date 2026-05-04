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
