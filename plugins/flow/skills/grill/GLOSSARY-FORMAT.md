# Glossary format

## Structure

```md
# {Context name}

{One or two sentences: what this context is and why it exists.}

## Language

**Order**:
{One or two sentences that define the term.}
_Avoid_: purchase, transaction

**Invoice**:
A request for payment sent to a customer after delivery.
_Avoid_: bill, payment request
```

## Rules

- **Be opinionated.** Several words for one concept: keep the best, list the others under `_Avoid_`.
- **Tight definitions.** One or two sentences. Say what it is, not what it does.
- **Project terms only.** General programming concepts (timeout, error type, utility) do not
  belong, even if the project uses them everywhere. Before adding a term: specific to this
  context, or generic? Only the former goes in.
- **Subheadings** when natural clusters appear. One cohesive area: a flat list.

## One or several contexts

**One context (the common case)**: a single file at the root.

**Several contexts**: a `CONTEXT-MAP.md` at the root lists the contexts, where they live and how
they talk to each other:

```md
# Context map

## Contexts

- [Ordering](./src/ordering/CONTEXT.md): receives and tracks orders
- [Billing](./src/billing/CONTEXT.md): issues invoices, takes payments

## Relationships

- **Ordering → Billing**: Ordering emits `OrderPlaced`, Billing consumes it
- **Ordering ↔ Billing**: shared `CustomerId` and `Money` types
```

Map present: read it to find the context. Root file only: one context. Neither: create the root
file with the first settled term.
