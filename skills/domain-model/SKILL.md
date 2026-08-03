---
name: domain-model
description: Create or update a project's domain model artifacts — the CONTEXT.md domain glossary at the repo root and Architecture Decision Records in docs/adr/. Use when the user wants to "create a domain model", "set up CONTEXT.md", "add a glossary term", "record an ADR", "document a decision", or when a design conversation surfaces a new domain concept or settles a question future work might otherwise re-litigate. Companion to improve-codebase-architecture, which reads these artifacts.
---

# Domain Model

Maintain the two artifacts that carry a project's domain knowledge:

- **`CONTEXT.md`** — the domain glossary at the repository root. [CONTEXT-FORMAT.md](CONTEXT-FORMAT.md) is normative for structure and rules.
- **`docs/adr/`** — Architecture Decision Records. [ADR-FORMAT.md](ADR-FORMAT.md) is normative for when to write one and the format.

## Workflow

1. **Determine which artifact the conversation calls for.**
   - A new concept, a renamed concept, or one term used two different ways → glossary work.
   - A decision that closes a door, encodes a non-obvious constraint, or would otherwise be re-derived from scratch → an ADR.
   - Some conversations produce both: the decision goes in an ADR; the vocabulary it introduced goes in the glossary.

2. **Read the relevant format doc before writing.** Both live in this skill's directory and are normative — follow their structure, rules, and "when to write" guidance exactly.

3. **For `CONTEXT.md`:**
   - If the file doesn't exist, create it from the structure in CONTEXT-FORMAT.md: a `## Domain` paragraph in domain terms (not technical terms), then `## Terms` with one `###` heading per term.
   - Starting fresh, interview the user: what are the nouns of this system? What does the team call them out loud?
   - Apply the rules: one term per heading, two-to-three-sentence definitions, cross-reference instead of repeating, no code or file paths, record the team's own words.

4. **For ADRs:**
   - Name the file `docs/adr/NNNN-short-kebab-title.md`, where `NNNN` is the next zero-padded number after the highest existing one.
   - Fill all four sections: Context, Decision (an active sentence), Consequences (costs stated honestly), Alternatives considered.
   - Never edit an accepted ADR's body — write a superseding ADR and mark the old one `Status: superseded by ADR-NNNN`.

5. **Update inline, not later.** Write the change in the same conversation that surfaced it — glossaries and decision logs that wait for "later" go stale and get ignored.
