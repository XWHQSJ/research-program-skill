# Research Program Skill

A portable research workflow skill for Claude Code, Codex CLI, and Cursor Agent.

It combines two useful ideas:

- `ml-intern`: discovery, evidence labeling, and trace discipline for research work
- `autoresearch`: baseline-first, metric-first, one experiment at a time

The default mode is one-shot. Run one baseline or one experiment, record the result, then stop.

## Install for Claude Code

```bash
mkdir -p ~/.claude/skills
cp -R skills/research-program ~/.claude/skills/research-program
```

Then invoke:

```text
/research-program
```

## Use with Codex CLI or Cursor Agent

Copy `examples/program.md` into the target repo, then tell the agent:

```text
Read program.md and follow it exactly. Run the baseline if none exists. Otherwise run one experiment only, record results.tsv, then stop.
```

## When to use

Use this for:

- paper, repo, dataset, model, benchmark, or docs discovery
- turning a research topic into a reproducible experiment plan
- creating a shared `program.md` that multiple coding agents can follow
- running controlled baseline and experiment cycles
- adapting `ml-intern` or `autoresearch` ideas without giving those tools separate model API keys

Do not use it for unrestricted autonomous loops. Continuous or overnight mode should require explicit user instruction.

## Repository layout

```text
skills/research-program/SKILL.md  # Claude Code skill
examples/program.md               # reusable research program template
examples/results.tsv              # result log example
```

## Safety model

The workflow keeps research bounded by requiring:

- a target metric before experiments
- a baseline before changes
- explicit allowed and forbidden files
- a `results.tsv` record for every run
- one run only unless the user explicitly asks for continuous mode

## License

MIT
