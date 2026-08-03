---
name: questions
description: Batch pending ambiguities into structured AskUserQuestion tool calls instead of asking them as plain text. Use when you have 2+ unresolved questions, before risky work where assumptions could bite, or for an end-of-conversation sweep for lingering unknowns.
allowed-tools: AskUserQuestion
---

# Questions Skill

Batch all pending ambiguities/assumptions/decision points into structured AskUserQuestion calls instead of inline text questions.

## When to use

- User runs `/questions`
- You have 2+ open questions about the current task
- Before risky work where wrong assumptions could cause rework
- End-of-conversation sweep for lingering unknowns

## When NOT to use

- Single simple yes/no — just ask inline
- Answer is obvious from context or code — derive it yourself

## Rules

1. **1–4 questions per AskUserQuestion call**, 2–4 options each (the UI auto-adds "Other")
2. Each option: **short label** (1–5 words) + **description** explaining the tradeoff
3. Put the recommended option **first** with `(Recommended)` suffix on the label
4. `header` = short tag, **max 12 chars** (e.g. "Scope", "Backend", "Auth")
5. `multiSelect: true` when choices aren't mutually exclusive
6. If you have **>4 questions**, use multiple sequential AskUserQuestion calls

## Process

1. **Scan** the current task/conversation for every ambiguity, assumption, and decision point
2. **Filter out** anything you can derive from the code, docs, or prior context
3. **Group** related questions (e.g. scope + approach can go in one call)
4. **Craft options** — each one concrete and actionable, not vague "Option A/B" labels
5. **Fire** the AskUserQuestion call(s)

## Common mistakes to avoid

- Asking questions as inline plain text when you have 2+ questions
- Vague labels like "Option A", "Approach 1" — use descriptive labels
- Missing descriptions on options — always explain the tradeoff
- Wasting question slots on things you could figure out yourself
- Forgetting `multiSelect: true` when choices can be combined
