---
name: explore
description: >
  Brainstorm a non-code decision (hardware to buy, infra to choose, a project to scope) until it
  settles, keeping the findings in topic sheets pushed to a repo. The non-code sibling of feature.
  Triggers — "/flow:explore", "brainstorm", "help me choose", "compare options", "explore an idea".
argument-hint: "What are we deciding?"
---

# Explore

Like `feature`, but the output is decisions written down, not code.

## 0. Settings

One question, before anything else: **where do the sheets live?** An existing repo, a new
private GitHub repo (`gh repo create <slug> --private`), or local files only. Once a repo
exists, every settled change is committed and pushed (skill `git:commit` with `push`) without
asking again.

## 1. Interview

Follow skill `flow:grill`: rounds, a recommendation per question, the glossary. On top of it:

- **Facts are researched, never asked.** Inspect the machine, read the user's repos and orgs
  (`gh`), search the web. Prices, availability and "latest model" questions always get a fresh
  search: they move by the month.
- **Date every price and mark it indicative.** Cite the sources at the bottom of the sheet.
- **A new fact reopens a settled decision.** Say which decision it breaks, re-run the comparison,
  and update the sheets. Do not defend the old choice.
- **Name the trade-off in user terms**: euros per year, watts, decibels, hours of their time,
  not just specs.
- **Flag dubious deals and sellers** when the user pastes one: check reviews before praising a
  price.

## 2. Sheets

Write as decisions settle, not at the end:

- `README.md`: the goal in two lines, the current choice (a shopping list, a decision table…)
  and an index of the sheets.
- `docs/<topic>.md`: one per topic. A comparison table with a ★ on the pick, the criteria above
  it, the caveats below it, its sources, and a link back to the README.
- `docs/piste-<topic>.md` for options explored and not retained: a status line at the top, the
  numbers, the verdict, and what would make it worth revisiting.
- A sheet for a third party when the decision needs someone else's yes (parents, a manager,
  a client): plain words, their concerns, the numbers that matter to them.

Keep the README short: the details live in the sheets.

## 3. End

When no question is left open, summarize the decisions in a few lines, give the repo link, and
name the next step: what the user does in the real world, and which skill picks up after it
(usually `flow:feature` once there is something to build).
