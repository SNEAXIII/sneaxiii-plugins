---
name: grill
description: >
  Grill the user on an idea or a plan until there is a shared understanding, writing the
  vocabulary and the decisions into the project docs as they settle.
  Triggers — "grill me", "challenge the idea", "stress-test this", "dig in before coding".
---

# Grill

Interview the user until nothing is silently assumed.

## In rounds

Every decision opens others. A round asks every question that can be asked **now**, without
depending on an answer not yet given. The others wait for a later round.

```
❓ **Q1 — <title>**: <the question, with the options if there are any>

➡️ <your recommendation>
```

Word each question so "yes" accepts your recommendation. Wait for the answers before the next round.

## Facts are your job

What can be read in the code or the tools, you look up (a subagent if it is long), you do not ask.
If the user states a behavior the code contradicts, show it. Decisions are the user's.

## The glossary

The project's vocabulary file: `CONTEXT.md`, or `GLOSSARY.md` if that one already exists. A
`CONTEXT-MAP.md` at the root means several contexts, each with its own file: read the map, infer
the topic's context, ask if it is ambiguous. Read the file before the first round.

During the interview:

- **Conflict** — a term used differently from the glossary: "The glossary defines X as A, you
  seem to mean B. Which is it?"
- **Fuzziness** — a vague or overloaded term: propose the precise one. "Account — do you mean
  the Player or the User? Those are different things."
- **Scenarios** — invent concrete edge cases that force the boundary between two concepts.
- **Write at once** — a settled term is written right away, not at the end of the session, in
  the format of [GLOSSARY-FORMAT.md](GLOSSARY-FORMAT.md). The file is created only with the
  first settled term.

The glossary is only that: not a spec, not a scratch pad, no implementation detail.

## ADRs

Offer an ADR only when all three hold:

1. **Hard to reverse** — changing your mind later has a real cost.
2. **Surprising without context** — a future reader will wonder "why this way?".
3. **A real trade-off** — there were genuine alternatives, picked for specific reasons.

If any one is missing, no ADR. Format and examples of what qualifies: [ADR-FORMAT.md](ADR-FORMAT.md).

## End

When no question is left open, summarize the decisions in a few lines and wait for the user to
confirm. Do not code anything before that.
