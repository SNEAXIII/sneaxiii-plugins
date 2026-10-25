---
name: merge-pr
description: >
  Merge an open PR into the default branch once CI is green: lists and sorts the PRs, watches the
  checks in the background and repairs a red check, squashes with a conventional title, deletes the
  branch and resyncs. Never for promoting the default branch to a release or staging branch.
user-invocable: true
---

# Merge PR

Merge an open PR into the default branch. **One PR per call.**
`<base>` = `gh repo view --json defaultBranchRef --jq .defaultBranchRef.name`.

To open a feature's PR: `/make-pr`.

## Picking the PR

`/merge-pr <n>`: the PR is given, skip the list (the summary confirmation is still due).

Bare `/merge-pr`: list and **sort**.

```bash
gh pr list --state open --base <base> --json number,title,author,isDraft,mergeable,mergeStateStatus,headRefName,statusCheckRollup
```

Two separate tables, **humans** then **bots** (`author.is_bot`, dependabot, renovate). No special
treatment for bots: the split only keeps the batch of updates from drowning the feature PRs.

In each table, two blocks:

| Block | Criterion |
| --- | --- |
| **ready to merge** | `mergeable == "MERGEABLE"`, not draft, green check rollup |
| **blocked** | conflict (`CONFLICTING`), draft, a red check, or checks still running |

Show number, title, author, branch, and for blocked ones **the reason**. Then ask which one to
merge. Never pick one by default, even when there is only one.

## Preflight — blockers

Blockers 1 to 3 are reported and stop the skill.

1. **PR base** — if `baseRefName != <base>`: it is a promotion, outside this skill.
2. **Draft** — stop, say it must leave draft.
3. **Conflict** — `mergeable == "CONFLICTING"`: stop, say a rebase on `<base>` is due.
4. **Checks**

   ```bash
   gh pr checks <n> --json name,state,link
   ```

   - Running (`PENDING`): start a **watch** in the background (`run_in_background`), with no cap,
     and **keep working** on the rest of the conversation. The completion notification resumes
     the skill at step 4.

     ```bash
     gh pr checks <n> --watch --interval 30 > /dev/null; gh pr checks <n> --json name,state,link
     ```

   - A `FAILURE`: **diagnose**. A GitHub Actions check's `link` holds `runs/<run-id>/job/<job-id>`:

     ```bash
     gh run view <run-id> --job <job-id> --log-failed
     ```

     The diagnosis is done when it names the red test or step, the exact error, and says whether
     the cause lies in the PR diff (`gh pr diff <n> --name-only`) or outside it (flaky, infra).
     Then repair:
     - cause in the diff → fix on the PR branch, commit, push;
     - cause outside → `gh run rerun <run-id> --failed`.

     Start the watch again afterwards. Second failure on the same check: report the check, its
     `link` and the cause, and stop. A check outside GitHub Actions (Sonar, Snyk…): give its
     `link` and stop.
   - A missing check is not a blocker: path-conditional checks are normal.
5. **Local worktree** — `git status --short` on the PR branch, right before the merge. A tracked
   file modified since the last push: stop and ask whether it belongs to the PR. The user often
   touches the branch between the push and the merge.

## Squash title

The squash takes the PR title; it is what ends up in the history and the changelog.

- Conventional title (`feat:`, `fix:`, `chore:`, `ci:`, `refactor:`, `docs:`, `test:`, `build:`,
  `style:`): keep it, and merge as soon as checks are green — `/merge-pr <n>` is the go-ahead.
- Otherwise: propose a corrected title following the type rules in `CLAUDE.md` (failing that, by
  what the end user sees, not by which files changed), and get it approved **before** starting
  the watch, so the rest runs on its own.

A command chained after the merge (`/release-pr`…) starts as soon as the merge is done.

## Run

1. **Squash**, never a merge commit — a merge commit repeats the PR title in its body, which a
   changelog generator then counts twice:

   ```bash
   gh pr merge <n> --squash --delete-branch --subject "<approved title>"
   ```

2. **Resync local**:

   ```bash
   git checkout <base> && git pull --prune
   ```

   If the worktree is dirty, force nothing: say so and let the user tidy up.

   If the PR branch lives in a linked worktree (`git worktree list`), `<base>` is checked out in
   the main checkout: pull there instead. Then offer to remove the worktree — dependency
   junctions or symlinks first (`rmdir` on Windows, `rm` on the link elsewhere), then
   `git worktree remove <path>`. Removing it with the links still in place deletes the main
   checkout's dependencies through them.

3. **If the repo uses release-please** (`release-please-config.json` present), report its state
   without starting anything:

   ```bash
   gh run list --workflow "Release Please" --branch <base> -L 1 --json status,conclusion,url
   gh pr list --state open --json number,title,url,headRefName \
     --jq '.[] | select(.headRefName | startswith("release-please--"))'
   ```

   Give the version the release-please PR carries.

## Rules

- One PR per call — no chained batch of bot PRs.
- Never `--admin`: a red check cannot be bypassed from here.
- Never `--force`, never a direct push to `<base>`.
- Never `--merge` or `--rebase`: squash only.
