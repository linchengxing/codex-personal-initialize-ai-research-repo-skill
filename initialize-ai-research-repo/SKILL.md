---
name: initialize-ai-research-repo
description: Initialize AI/ML research code repositories by creating or updating Agent.md with operational context and experiment workflow rules. Use when Codex is asked to set up a research repo, write repo-specific agent instructions, define conda environment and GPU allocation for training/evaluation, or establish experiment records separate from running logs with readable experiment naming and per-experiment markdown analysis.
---

# Initialize AI Research Repo

## Overview

Create or update `Agent.md` at the repository root so future agents know how to work in an AI research repo: which conda environment to use, which GPUs are reserved for training and evaluation, and how to record experiments without mixing human analysis with raw running logs.

## Workflow

1. Inspect the repository before writing:
   - Read existing `Agent.md`, `AGENTS.md`, `README*`, environment files, launch scripts, configs, and existing log/output directories.
   - Preserve existing repo-specific instructions. Merge instead of replacing unless the user explicitly asks for a rewrite.
   - Prefer the repo's existing names for raw log roots, output roots, checkpoints, and analysis folders when they are clear.

2. Collect required runtime facts interactively:
   - Require an exact conda environment name or path.
   - Require GPU IDs for training.
   - Require GPU IDs for evaluation/inference.
   - If any required value is missing or ambiguous, ask the user before finalizing `Agent.md`. Use `request_user_input` when available; otherwise ask concise plain-text questions.
   - Do not invent GPU assignments. Do not leave placeholders such as `TODO`, `<env>`, or `ask user` for these required facts in the final file.

3. Choose experiment workflow paths:
   - Define a human-authored experiment record root separate from raw running logs. Default to `experiments/` if the repo has no clear convention.
   - Define a raw running log root. Prefer an existing convention such as `runs/`, `outputs/`, `work_dirs/`, `logs/`, `wandb/`, or a framework-specific directory. Default to `runs/` only when no convention exists.
   - State that analysis, notes, figures, tables, and interpretations live under the experiment record directory, not the raw running log directory.

4. Write or update `Agent.md`:
   - Use the exact filename `Agent.md` unless the user asks for another file.
   - Keep commands and repo policies concrete enough that the next agent can execute them.
   - Include the sections below unless an existing structure makes a merged version clearer.

## Required Agent.md Content

Include these operational facts:

```markdown
## Runtime Environment

- Conda environment: `<exact-env-name-or-path>`
- Training GPUs: `<gpu-ids>`
- Evaluation GPUs: `<gpu-ids>`
- Before running training or evaluation, activate the conda environment and set `CUDA_VISIBLE_DEVICES` according to the task type.
```

Include this experiment workflow, adapted to the repo:

```markdown
## Experiment Workflow

- Experiment record root: `experiments/`
- Raw running log root: `runs/`
- Keep human-written records, hypotheses, code-change summaries, result interpretation, figures, and follow-up analysis under the experiment record root.
- Keep raw stdout/stderr logs, checkpoints, TensorBoard/W&B artifacts, and generated run outputs under the raw running log root.
- Every experiment must have one readable experiment name and one markdown record linked to its running log.
```

Define a readable naming rule:

```markdown
### Experiment Naming

Use names that can be understood at a glance:

`YYYYMMDD-HHMM_<task-or-dataset>_<model-or-method>_<main-change>_<seed-or-budget>`

Examples:
- `20260525-1430_cifar10_resnet50_augmix_s1`
- `20260525-1615_imagenet_vit-b_ablate-posenc_s42`

Avoid names like `exp1`, `test`, `debug-final`, or names that omit the task and main change.
```

Define the per-experiment record layout:

```markdown
### Per-Experiment Record

For each experiment, create:

`experiments/<experiment-name>/exp.md`

The experiment directory may also contain `analysis/`, `figures/`, `tables/`, and follow-up markdown notes.

The `exp.md` file must include:
- Experiment name
- Status
- Hypothesis or purpose
- Main code/config/data changes
- Command or config entry point
- Conda environment and GPU assignment used
- Link to raw running log directory, for example `../../runs/<experiment-name>/`
- Key metrics and result summary
- Analysis notes and next actions
```

## Interaction Rules

- Ask for missing conda/GPU details before writing the final `Agent.md`.
- If only experiment paths are missing, choose the defaults above and mention the choice.
- If the user provides repo-specific path preferences, use them exactly.
- If the repository already has raw run logs, do not move or rename them during initialization unless explicitly requested.
- Do not start training or evaluation as part of initialization unless the user asks.

## Validation

Before finishing, verify:

- `Agent.md` exists at the repo root.
- It contains the exact conda environment.
- It contains explicit training GPU IDs and evaluation GPU IDs.
- The experiment record root and raw running log root are distinct.
- The experiment naming convention includes task/dataset, method/model, main change, and seed/budget or an equivalent disambiguator.
- The per-experiment markdown record links to the raw running log directory.
