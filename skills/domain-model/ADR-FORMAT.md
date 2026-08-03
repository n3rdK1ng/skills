# ADR Format

Architecture Decision Records live in `docs/adr/`. Each ADR captures a decision and the reasoning that makes it load-bearing. ADRs exist so future explorers don't re-litigate a settled question.

## When to write one

Write an ADR when a decision:

- Closes a door other explorers will plausibly want to re-open
- Encodes a constraint that isn't obvious from the code
- Would otherwise have to be re-derived from scratch every time someone asks "why don't we do X?"

Don't write an ADR for:

- Decisions self-evident from the code
- Ephemeral choices ("not worth it right now")
- Style preferences already covered by CLAUDE.md or lint rules

## File naming

`docs/adr/NNNN-short-kebab-title.md`, zero-padded sequence. Example: `docs/adr/0007-no-orm-in-export-pipeline.md`.

## Format

````markdown
# ADR-NNNN: <Decision title>

- **Status:** accepted | superseded by ADR-NNNN | deprecated
- **Date:** YYYY-MM-DD

## Context

What was the situation that forced a decision? What constraints were in play? Two or three paragraphs — enough that a future reader can reconstruct the world the decision was made in.

## Decision

What we decided, stated as an active sentence: "We will use raw SQL for the export pipeline."

## Consequences

What this decision buys, what it costs, and what doors it closes. Write the costs honestly — an ADR that only lists benefits is selling, not deciding.

## Alternatives considered

Each alternative that was on the table, and why it lost. This is the section future explorers come back to: "we already considered X; here's why it didn't work."
````

## Updating an ADR

ADRs are append-only in spirit. If a decision changes:

- Mark the old ADR `Status: superseded by ADR-NNNN`
- Write a new ADR that references the old one in its Context section

Don't edit the body of an accepted ADR — the record loses its value if it silently changes. A trail of superseded ADRs tells future explorers what the team has tried and learned; a rewritten ADR tells them nothing.
