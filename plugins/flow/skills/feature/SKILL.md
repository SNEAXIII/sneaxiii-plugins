---
name: feature
description: >
  Take a feature from idea to PR: interview with docs, plan, task-by-task execution by subagents,
  PR. Triggers — "/flow:feature", "start a feature", "from idea to PR".
disable-model-invocation: true
---

# Feature

## 0. Settings

Before anything else, one question in three parts:

- **Worktree** — work in a worktree, or on a branch of the current checkout?
- **Push** — push after each commit, or never without asking?
- **Ponytail** — run `ponytail:ponytail-review` on the branch once it is done?

The answers hold for the whole session.

## 1. Interview

Skill `flow:grill`. Done when the user confirms the summary of decisions.

## 2. Plan

Skill `flow:plan`. Done when the user approves the plan.

## 3. Execute

**Workspace.** On the default branch, create a `<type>/<slug>` branch. With a worktree: the native
tool (`EnterWorktree`) or `git worktree add`, then link the dependency folders from the main
checkout (`node_modules`, `.venv`…) instead of installing: junction `mklink /J` on Windows (same
volume), symlink elsewhere. A link shares the environment: if a task touches a lockfile, warn
before any `npm install` / `uv sync`, which would write into the main checkout.

For each `### Task N`, in order (one working tree, never in parallel):

1. **Implement** — a subagent gets the plan path, the task text, the Global Constraints and the
   workspace path. A project agent if one fits (`.claude/agents/`), otherwise `general-purpose`.
   Instruction: implement, run the relevant tests, do not commit, return the list of files touched.
2. **Review** — another subagent (the project's reviewer if there is one) gets the task and
   `git diff`. It answers OK, or a list of precise problems.
3. **Fix** — problems go back to the implementer (`SendMessage`), then a new review.
   On the second rejection, stop and show the problems to the user.
4. **Commit** — skill `git:commit`, with `push` only if the settings say so.

One progress line per finished task. At the end, summarize what was done and what departed from
the plan.

## 4. Ponytail

If the settings say so: skill `ponytail:ponytail` with `ultra`, then skill
`ponytail:ponytail-review` on `git diff <base>...HEAD`. Show the findings, apply the ones the user
keeps, then commit them like a task.

## 5. PR

Ask for the go-ahead, then skill `git:make-pr`. Stop on the PR link: merging is `git:merge-pr`,
on request. Without the go-ahead, end on the push command in a code block.
