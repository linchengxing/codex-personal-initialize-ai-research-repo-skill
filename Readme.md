# Initialize AI Research Repo Skill

This repository contains a Codex skill for initializing AI research code repositories. It creates or updates `Agent.md` with repo-specific runtime details, GPU allocation rules, and a research experiment workflow.

The skill is manual-only. Invoke it explicitly with:

```text
$initialize-ai-research-repo
```

## Install

Clone this repository:

```bash
git clone git@github.com:linchengxing/codex-personal-initialize-ai-research-repo-skill.git
cd codex-personal-initialize-ai-research-repo-skill
```

Install by copying the skill folder into Codex's personal skills directory:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R initialize-ai-research-repo "${CODEX_HOME:-$HOME/.codex}/skills/"
```

Alternatively, install with a symlink so local repository edits are picked up directly:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
ln -sfn "$PWD/initialize-ai-research-repo" "${CODEX_HOME:-$HOME/.codex}/skills/initialize-ai-research-repo"
```

Restart Codex or reload skills if the new skill is not immediately visible.

## What It Produces

The skill initializes an `Agent.md` at the target AI research repository root. It requires the user to provide:

- the exact conda environment
- GPU IDs for training
- GPU IDs for evaluation

It also defines `codex_exp_log/` as the default human-written experiment record root, separate from raw running logs such as `runs/`, `outputs/`, `work_dirs/`, or framework-specific output directories.
