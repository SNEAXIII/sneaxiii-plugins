---
name: handoff
description: >
  Compact the current conversation into a handoff document a fresh session can pick up.
  Triggers — "/flow:handoff", "handoff", "hand this off", "write a handoff".
argument-hint: "What will the next session be used for?"
disable-model-invocation: true
---

# Handoff

Write a document that lets a fresh session continue the work without this conversation.

## Where

`docs/handoff/<YYYY-MM-DD>-<slug>.md` in the current repo. If the folder does not exist, create it
with a `.gitignore` containing `*`, so handoffs never get committed.

## Content

- **Goal** — what the work is for, in one or two sentences.
- **Done** — what is finished, by reference.
- **State** — for each repo touched (submodules and nested repos included): branch, worktree
  path, uncommitted files, commits not pushed, open PR. Read it with git, do not recall it.
- **Decisions** — what was settled and why, especially what the next session might undo.
- **Dead ends** — what was tried and failed, so it is not tried again.
- **Next** — the next concrete steps, in order.
- **Skills** — which skills the next session should call, and when.

## Rules

- Reference what already lives elsewhere (plan, ADR, glossary, commit, PR, issue) by path or URL;
  never copy it.
- Redact secrets, tokens and personal data.
- Arguments describe what the next session is for: shape the document around that.

## End

Give the path, then the line to paste in the new session:

```
Read <path> and continue the work.
```
