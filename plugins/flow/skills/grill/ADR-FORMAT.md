# ADR format

In `docs/adr/`, numbered `0001-slug.md`, `0002-slug.md`…: take the highest existing number + 1.
The folder is created with the first ADR. If the project already has ADRs, follow their shape.

## Template

```md
# {Short title of the decision}

{1 to 3 sentences: the context, what was decided, why.}
```

That's it. An ADR can be a single paragraph: its value is recording *that* a decision was made
and *why*, not filling in sections.

## Optional sections

Only when they add something:

- **Status** (`proposed | accepted | deprecated | superseded by ADR-NNNN`): when the decision is revisited
- **Considered options**: when the rejected alternatives are worth remembering
- **Consequences**: when a downstream effect is not obvious

## What qualifies

- **Architectural shape.** "Monorepo." "Event-sourced writes, reads projected into the database."
- **Integration between contexts.** "Ordering and Billing talk through events, not synchronous HTTP."
- **Technology choices with lock-in.** Database, bus, auth provider, deployment target. Not every
  library: the ones that would take a quarter to replace.
- **Boundaries and scope.** "Customer data belongs to the Customer context; the others keep only
  the ID." The no's matter as much as the yes's.
- **Deliberate departures from the obvious path.** "Hand-written SQL rather than an ORM, because
  X." Anything a reasonable reader would assume the opposite of: it stops them from "fixing" a
  deliberate choice.
- **Constraints invisible in the code.** "No AWS, for compliance." "Under 200 ms, partner API contract."
- **Alternatives rejected for a non-obvious reason.** Otherwise someone will suggest them again
  in six months.
