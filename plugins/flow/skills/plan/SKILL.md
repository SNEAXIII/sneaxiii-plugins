---
name: plan
description: >
  Write the implementation plan of a multi-step request, before touching code.
  Triggers — "make a plan", "write the plan", "plan this", or a request big enough to split into tasks.
---

# Plan

A plan gives instructions, not code. The implementer is competent: tell them what to do, where,
and how to know it is done. They write the code.

Read the code involved before writing the plan: the instructions build on what exists
("reuse `X`", "follow the pattern of `Y`").

## Where

`docs/plans/YYYY-MM-DD-<feature>.md`, unless the project already has a folder for plans.

## Format

```markdown
# <Feature>

**Goal:** one sentence.

## Global Constraints

- one line per rule that holds for every task

---

### Task 1: <name>

**Files:** `path/a.py`, `path/b.tsx`

What to do, in a few sentences: the expected behavior, what to reuse, the known pitfalls.

**Done when:** the observable behavior, or the test that proves it.
```

## Rules

- No code block, except an exact imposed value (a string, a regex, a format).
- Each implementer sees only their own task: if a task creates a name another one uses
  (endpoint, function, i18n key), name it in both.
- One task = one deliverable a reviewer could reject on its own. No standalone "config" task.
- No line that decides nothing ("handle edge cases", "TBD").

## Before handing over

Every requirement of the request has its task. Then give the plan's path and wait for the review.
