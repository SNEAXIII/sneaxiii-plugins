---
name: make-pr
description: >
  Open the PR of a feature branch against the default branch: commit, push, `gh pr create`.
  Never for promoting the default branch to a release or staging branch.
user-invocable: true
---

# Make PR

Full workflow to open a PR against the default branch from the current branch.
`<base>` = `gh repo view --json defaultBranchRef --jq .defaultBranchRef.name`.

## Steps

1. **Branch** — on `<base>`, first create a feature branch (`<type>/<slug>`):
   the commit skill stops on the default branch.

2. **Commit and push** through the `git:commit` skill with the `push` argument — never a
   hand-made commit. It splits the changes into commits by responsibility, then pushes the
   branch (`git push -u origin HEAD`) once every commit is made.

3. **Create the PR** with the `gh` CLI:
   ```bash
   gh pr create --base <base> --head <branch> --title "<title>" --body ""
   ```
   - Several remotes (a fork and its upstream) and no `gh repo set-default`: ask which repo the
     PR targets, then pass it with `--repo <owner>/<name>`.
   - Empty or minimal body — no Summary / Test plan sections.
   - If `gh` answers `HTTP 401: Bad credentials`, ask the user to run `gh auth login`, then
     run the command again.

## Args

If args are given (e.g. `/make-pr fix login redirect`), use them as the PR title.
Otherwise, derive the title from the branch commits (`git log <base>..HEAD --oneline`).

## Notes

- If there is nothing to commit and the branch has no commit ahead of `<base>`, tell the user
  and stop.
- Never force a push (`--force`).
- No hand-run lint or format: the repo's pre-commit hooks handle it at commit time.
