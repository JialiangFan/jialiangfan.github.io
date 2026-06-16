---
title: "A Practical Structure for Research Codebases"
date: 2026-06-16
permalink: /blog/research-project-structure/
categories:
  - blog
tags:
  - research-engineering
  - reproducibility
  - robot-learning
excerpt: "A compact project structure for keeping AI and robotics experiments readable, reproducible, and easier to hand off."
related: false
---

Research codebases often fail for boring reasons. The method may be interesting, the experiments may be expensive, and the paper figure may look clean, but the project itself becomes hard to inspect: goals live in scattered notes, scripts assume local paths, checkpoints are separated from configs, and no one can quickly tell which result is the current one.

I keep coming back to one practical rule: separate intent, implementation, and evidence.

```text
docs/   What we are trying to do, why it matters, and what counts as done.
code/   The implementation: source code, scripts, configs, and tests.
runs/   The evidence produced by execution: configs, logs, metrics, artifacts.
```

This structure is simple, but it is useful for research projects that involve training, evaluation, multiple seeds, simulators, real-world validation, ablations, and leaderboard-style result aggregation.

## The Minimal Layout

A small research project does not need many folders, but it does need clear responsibility boundaries.

```text
project/
  README.md
  docs/
  src/
  scripts/
  configs/
  runs/
  tests/
```

For larger projects, I usually add a few more directories:

```text
project/
  README.md
  docs/
  src/
  scripts/
  configs/
  runs/
  data/
  tests/
  tools/
  notebooks/
  assets/
```

The important part is not the exact folder count. The important part is that each folder has one job.

- `README.md` gives the project overview, quick start, main commands, and links to the important documents.
- `docs/` stores experiment specifications, task breakdowns, design decisions, status tracking, and blockers.
- `src/` stores reusable implementation logic.
- `scripts/` stores thin command-line entry points for training, evaluation, aggregation, and maintenance.
- `configs/` stores reusable training, evaluation, model, environment, and backend configs.
- `runs/` stores experiment outputs, including resolved configs, checkpoints, logs, metrics, raw results, and artifacts.
- `data/` stores dataset manifests, metadata, download scripts, examples, and format notes. Large datasets usually should not be committed to git.
- `tests/` protects the pieces that make results credible: config loading, path conventions, metrics, data formats, checkpoint loading, and smoke runs.
- `tools/` stores project maintenance utilities such as result aggregation, validation, conversion, and cleanup.
- `notebooks/` is for exploration. Stable logic should move into `src/`, `scripts/`, or `tools/`.
- `assets/` stores figures, diagrams, demo images, GIFs, and paper-facing visual material.

I avoid creating separate top-level `outputs/`, `results/`, `logs/`, or `checkpoints/` folders unless there is a strong reason. Most of those files belong under `runs/`, where they can stay attached to the run that produced them.

## Make `docs/` the Source of Truth

The `docs/` directory should not be a loose note dump. It should define the work clearly enough that a collaborator, future self, or coding agent can continue without guessing.

For each major experiment, I use a structure like this:

```text
docs/
  specs/
    <experiment_name>/
      README.md
      tasks/
        01_<task_name>.md
        02_<task_name>.md
        03_<task_name>.md
      decisions.md
      changelog.md
```

The experiment `README.md` should answer the high-level questions:

- What is the goal?
- Why does this experiment matter?
- What is in scope, and what is explicitly out of scope?
- Which environment or backend is involved?
- Which methods are being compared?
- What sub-tasks are required?
- What are the success criteria?

For AI and robotics projects, the environment field matters. A task may run in an MVP environment, a simulator, or a real-world setup. The document should name the backend directly, such as MuJoCo, Isaac Gym, RoboCasa, LIBERO, ABB GoFa, UMI, or any other system that determines how the code actually runs.

Each sub-task gets its own markdown file. That file should state the status, objective, background, technical details, inputs, outputs, code paths, reproduction command, expected `runs/` path, metrics, acceptance criteria, blockers, and notes.

The most useful habit is to write the acceptance criteria before treating the task as done. A task is not done just because code exists. It is done when the implementation exists, the result can be reproduced, the output is written to the documented `runs/` path, the required config and metrics files exist, and the main result has been checked.

Use a small status vocabulary:

```text
Planned      not started
In Progress  implementation or experiment is ongoing
Blocked      blocked by a concrete missing dependency or decision
Done         implemented, reproduced, and documented
Deprecated   no longer used
```

This keeps project state readable. More importantly, it makes blockers concrete instead of vague.

## Treat `runs/` as the Evidence Layer

The `runs/` directory stores what actually happened. It should not explain the experiment design; that belongs in `docs/`. Instead, `runs/` should preserve the artifacts needed to inspect, reproduce, aggregate, and debug results.

I use a flat run structure:

```text
runs/
  <method>/
    <run_id>/
      run_config.json
      train/
        train_config.json
        checkpoints/
        logs/
        artifacts/
      eval/
        <eval_id>/
          eval_config.json
          metrics.json
          raw_results/
```

The `run_id` should be unique and somewhat readable:

```text
<date>_<time>_<backend>_<model>_<env>_s<seed>
```

For example:

```text
20260612_1530_isaacgym_dp_pickplace_s0
20260612_1715_mujoco_mlp_lift_s1
20260613_1015_real_gofa_dp_pickplace_s0
```

The `run_id` is only a label. It should not be the only metadata source. Each run should contain a `run_config.json` with the method, backend, backend name, model type, environment name, seed, creation time, and any project-specific notes.

For very different execution settings, I prefer explicit backend fields:

```json
{
  "backend": "real_world",
  "backend_name": "abb_gofa"
}
```

This supports MVP validation, simulator training, real-world evaluation, sim-to-real transfer, backend-specific models, and cross-backend evaluation for the same model.

## Keep Evaluation Results Machine-Readable

Evaluation should be stored under:

```text
runs/<method>/<run_id>/eval/<eval_id>/
```

One training run can have many evaluations:

```text
runs/
  icrl/
    20260612_1530_isaacgym_dp_pickplace_s0/
      eval/
        success_isaacgym_pickplace_ckpt100_s0/
        robustness_isaacgym_noise0.1_pickplace_ckpt100_s1/
        transfer_real_gofa_pickplace_ckpt100_t0/
```

Each evaluation directory should contain:

```text
eval_config.json
metrics.json
raw_results/
```

The `eval_config.json` records the resolved evaluation parameters. The `metrics.json` records summary metrics used by tables, leaderboards, and comparisons. The `raw_results/` directory keeps the lower-level evidence: episodes, trajectories, logs, videos, or other artifacts needed for debugging and inspection.

The format of raw results can vary across projects, but `metrics.json` should stay stable. That stability makes automatic aggregation possible. A leaderboard script can recursively scan:

```text
runs/*/*/eval/*/metrics.json
```

Then it can combine each metrics file with the corresponding `run_config.json` and `eval_config.json` to build a unified result table.

Evaluation scripts should support either a direct run directory:

```bash
python evaluate_icrl.py \
  --run_dir runs/icrl/20260612_1530_isaacgym_dp_pickplace_s0
```

or a `run_id` plus a root directory:

```bash
python evaluate_icrl.py \
  --run_id 20260612_1530_isaacgym_dp_pickplace_s0 \
  --runs_root runs/icrl
```

The script should locate the run, read `run_config.json`, generate a unique `eval_id`, create the evaluation directory, write `eval_config.json`, write `metrics.json`, and save raw result files.

## A Simple Workflow

The working loop is:

1. Write the experiment specification in `docs/`.
2. Break the experiment into concrete sub-tasks.
3. Give each task a standalone markdown file.
4. Implement the code against the specification.
5. Save all execution outputs under `runs/`.
6. Update the documentation with actual code paths, commands, and result paths.

This loop is intentionally strict. Research projects change quickly, and that is fine. The structure is there so that changes leave a trace: what changed, why it changed, which command produced which result, and where the evidence lives.

## Why This Helps

This layout makes a project easier to inspect, reproduce, extend, and hand off.

It also makes the project easier to operate with coding agents. A good task file gives the agent a goal, input-output contract, implementation location, reproduction command, metrics, result path, and acceptance criteria. That is much better than asking the agent to infer the project plan from scattered scripts and old notebooks.

The structure is not meant to replace dataset versioning, model release systems, or online experiment trackers. It is a local project discipline: a compact way to keep research intent, implementation, and evidence connected.

The source guide is available here: [JialiangFan/research-project-guide](https://github.com/JialiangFan/research-project-guide).
