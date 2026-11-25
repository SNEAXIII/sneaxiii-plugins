---
name: explore
description: >
  Brainstorm a decision or a topic (hardware to buy, news to follow, a technology to watch, a
  concept or project to scope) until it settles, keeping the findings in topic sheets pushed to a
  repo. The general sibling of feature, whose interview stays on the code side.
  Triggers — "/flow:explore", "brainstorm", "help me choose", "compare options", "explore an idea",
  "tech watch".
argument-hint: "What are we deciding?"
---

# Explore

Like `feature`, but the output is decisions written down, not code.

## 0. Settings

Before anything else, one question in two parts:

- **Sheets** — where do they live? An existing repo, a new private GitHub repo
  (`gh repo create <slug> --private`), or local files only. Once a repo exists, every settled
  change is committed and pushed (skill `git:commit` with `push`) without asking again.
- **Scope** — what kind of topic? Hardware or a purchase, news, tech watch, a concept or a
  project to scope, or something else. It decides the research below.

## 1. Interview

Follow skill `flow:grill`: rounds, a recommendation per question, the glossary. On top of it:

- **Research beyond the code**: inspect the machine, read the user's repos and orgs (`gh`).
  Cite the sources at the bottom of the sheet.
- **By scope:**
  - *Hardware or a purchase* — date every price and mark it indicative. Name the trade-off in
    user terms: euros per year, watts, decibels, hours of their time, not just specs. Flag
    dubious deals and sellers when the user pastes one: check reviews before praising a price.
  - *News* — date every fact, prefer primary sources, and say what is still unconfirmed.
  - *Tech watch* — current versions and release dates from a fresh search, maturity (adoption,
    maintenance, licence), and what changed since the user last looked.
  - *Concept or project* — inspect a real sample before asking (a data export, an API page, the
    existing tools), and say what already exists and what it lacks.

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
(usually `flow:feature` once there is something to build; its interview then reads these sheets
and only covers the code side).
