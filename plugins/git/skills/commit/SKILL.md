---
name: commit
description: Use when ready to commit changes, or to commit and push — analyzes git status and diff, groups changes by responsibility, creates separate conventional commits in the right order with Co-Authored-By trailer. Pass `push` to push once every commit is made, and `default` (or `def`) to work straight on the default branch.
model: claude-haiku-5-5
---

# Git Commit

Analyze changes, group by responsibility, commit with conventional messages.

## Arguments

| Argument | Effect |
|----------|--------|
| _(none)_ | Commits only, nothing is pushed |
| `push` | Commits, then `git push` once **every** commit is made |
| `default` / `def` | Commit straight on the repo's default branch, whatever its name (`main`, `master`…) |

They combine: `/commit push def` commits and pushes on the default branch.

`<default>` = `git symbolic-ref --short refs/remotes/origin/HEAD` without its `origin/` prefix.

Any other argument is a grouping instruction in plain language (`/commit just the tests`), not a flag.

## Process

0. **Check the branch first** — `git branch --show-current` and
   `git rev-list --left-right --count @{u}...HEAD` (skip if there is no upstream).
   State the branch and how far it is from its upstream. If it is the wrong branch
   (see _Rules_), **stop here** and ask — commit nothing, switch nothing, create no branch
   on your own.
1. **Light overview first** — never the full raw `git diff`:
   - `git status --short`
   - `git diff --stat` (files and volume, without loading the content)
2. Targeted diff **only when needed** to decide the grouping:
   - `git diff -- <file>` on one file, not the whole working tree
3. Spot changes unrelated to the main feature → separate commits
4. Commit each group in the right order (fixes before features)
5. Check that each commit passes the pre-commit hook before moving on
6. With the `push` argument, push once, at the end:
   `git push -u origin HEAD` (`-u` covers the first push of a new branch and does no harm after).
   If the target is ambiguous — several remotes (`git remote -v`) and no upstream yet, or changes
   spanning a submodule or a nested repo — ask which repo and remote to push to before pushing.

## Conventional Commit Types

The type rules in the repo's `CLAUDE.md` win. Otherwise, Conventional Commits: pick the type by
what the end user sees, not by which files changed.

## Rules

- **Always** commit from a dedicated branch. The "wrong branch" is: `<default>`,
  `dev` / `develop`, a protected branch listed in `CLAUDE.md`, or a detached HEAD.
- In that case: **warn and ask**, never decide alone. State the current branch and why it is a
  problem, then offer the choice:
  1. create a dedicated branch — suggest a `type/topic` name drawn from the changes
  2. switch to an existing branch — name it
  3. stay here and commit anyway
  Wait for the answer. **Never** `checkout -b`, `switch` or `stash` without explicit consent: it
  moves the user's work without them asking.
- The `default` / `def` argument is prior consent to commit on `<default>`, without asking. It does not
  switch branches: if the current branch is not `<default>`, say so and ask.
- **Never** `git add .` or `git add -A` — always specific files
- **Separate commits** for unrelated changes
- Check for sensitive files before staging (`.env`, `*.key`, secrets)

## Template

```bash
git add path/to/file1 path/to/file2
git commit -m "type: short description"
```

## Grouping

- i18n files → same commit as the feature that needs them
- Manifest + lockfile (`package.json`/`package-lock.json`, `pyproject.toml`/`uv.lock`…) → same
  commit as the feature that installs the dependency
- Bug fix found along the way → separate commit **before** the feature
- Unrelated visual changes → separate `fix:` or `style:` commit
