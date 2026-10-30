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
   Already merged (`gh pr list --head <branch> --state merged` not empty): a squash left its
   commits off `<base>`, so a new PR replays them and conflicts. Stop and offer a fresh branch
   from `origin/<base>` with only the commits made since that merge, cherry-picked.

2. **Commit and push** through the `git:commit` skill with the `push` argument — never a
   hand-made commit. It splits the changes into commits by responsibility, then pushes the
   branch (`git push -u origin HEAD`) once every commit is made.

3. **Create the PR** with the `gh` CLI:
   ```bash
   gh pr create --base <base> --head <branch> --title "<title>" --body "<description>"
   ```
   - Several remotes (a fork and its upstream) and no `gh repo set-default`: ask which repo the
     PR targets, then pass it with `--repo <owner>/<name>`.
   - Description: 1 to 3 sentences on what changes and why, drawn from the branch commits — no
     Summary / Test plan headings, no checklist.
   - If `gh` answers `HTTP 401: Bad credentials`, ask the user to run `gh auth login`, then
     run the command again.

## Args

The title is always a full conventional commit header, `<type>: <description>` — the squash
commit reuses it. Derive it from the branch commits (`git log <base>..HEAD --oneline`).

- Args that read as a title (`/make-pr fix login redirect`) → `fix: login redirect`.
- A bare type (`/make-pr chore`) is the type only: keep it, derive the description.

## Notes

- If there is nothing to commit and the branch has no commit ahead of `<base>`, tell the user
  and stop.
- Never force a push (`--force`).
- No hand-run lint or format: the repo's pre-commit hooks handle it at commit time.
