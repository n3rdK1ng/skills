---
name: clickup-task
description: >
  Use this skill whenever the user writes "clickup task" or wants to create or update a ClickUp task
  from a Freelo task description or from scratch. This includes migrating single tasks, merging multiple
  Freelo tasks into one ClickUp task, updating the description of an existing ClickUp task, or creating
  brand new tasks unrelated to Freelo. Trigger any time the user pastes a Freelo task description and
  mentions ClickUp, provides a ClickUp task URL alongside task content they want written up, or asks
  to create a ClickUp task without any Freelo content.
allowed-tools: AskUserQuestion, mcp__claude_ai_ClickUp__clickup_create_task, mcp__claude_ai_ClickUp__clickup_update_task, mcp__claude_ai_ClickUp__clickup_get_task, mcp__claude_ai_ClickUp__clickup_add_tag_to_task, mcp__claude_ai_ClickUp__clickup_get_custom_fields, mcp__claude_ai_ClickUp__clickup_get_list, mcp__claude_ai_ClickUp__clickup_filter_tasks, mcp__claude_ai_ClickUp__clickup_resolve_assignees, mcp__claude_ai_ClickUp__clickup_find_member_by_name
---

# ClickUp Task Skill

This skill handles creating and updating ClickUp tasks — either migrating from Freelo or creating new tasks from scratch — following a strict format and approval workflow.

## Step 1: Parse the request

Determine from the conversation:
- Is there a **ClickUp task URL** already provided? If yes, this is an update to an existing task. If no, you'll be creating a new one.
- Is there **Freelo task content or a Freelo link** provided? If not, immediately ask the user: **"Is this a task migrated from Freelo, or a brand new task unrelated to Freelo?"** — and wait for the answer before proceeding.
- Are there **multiple Freelo items** being merged into one ClickUp task?

## Step 2: Ask all required questions upfront

Use `AskUserQuestion` to ask all of the following in a single call — do not skip any:

1. **Task type** — Bug or Normal?
2. **Assignee** — Who should be assigned? (If updating an existing task, pull the current assignee from ClickUp and ask to keep or change)
3. **Status** — What status should the task have?
4. **Tag(s)** — What tags should be applied?
5. **Time estimate** — Is there a time estimate? (In Freelo, this appears as a number with a `$` sign — if present, include it automatically and mention it to the user)
6. **Teamspace / List location** — Only ask this if creating a new task (offer the locations listed in `LOCAL.md` if present; otherwise ask the user where the task belongs)

## Step 3: Fetch the Notion template

Read `LOCAL.md` in this skill's directory. If it exists, it lists the Notion
template page ID for each task type. Based on the task type answer, fetch the
correct template **before drafting anything** using the Notion MCP
`notion-fetch` tool, and use its structure as the basis for the description.

If `LOCAL.md` does not exist, skip the fetch and use the template structures
in Step 5 directly (or ask the user for their own template page IDs if they
have them).

## Step 4: Fetch the existing ClickUp task (if updating)

If a ClickUp task URL was provided, use `clickup_get_task` to retrieve the current task details — description, assignees, tags, status, priority. This helps you preserve what should stay and only change what's needed.

## Step 5: Draft the description

Write the description in English using the Notion template structure. Follow these rules strictly:

### Formatting rules (apply to ALL tasks)

- **Headings**: All section headings (Summary, Description, Problem, Context, etc.) must use `**bold**` (e.g. `**Summary**`) — ClickUp does NOT render `##` markdown headings via the API, they appear as literal text
- **Lists**: Anything that looks like it could be bulleted — steps, options, behaviors, requirements — should be a bulleted list (`-`)
- **QA / Repro steps**: Always a numbered list with bulleted sub-steps under each scenario
- **No emojis** in the description content
- **Links section**: Always include at the end, even if empty, formatted as a bulleted list
- **Bug task names**: Always include the bug emoji at the start of the name
- **No original Freelo text** in the description — rewrite into clean English
- **Czech terms**: Never translate Czech names or domain-specific terms from the Freelo task (e.g. `rezie`, project names, role names, etc.) — keep them exactly as written

### API write/read behaviour

- **Writing**: Always use the `markdown_description` field (not `description`) when creating or updating tasks — this is the only field that preserves formatting
- **Reading**: The API strips formatting on read, so you cannot verify headings via API response. Direct the user to check the ClickUp UI to confirm headings and lists rendered correctly

### Normal task template structure

```
**Summary**

[Plain English description of the change/feature]

**QA Reproduction Steps**

1. **[Scenario name]**
   - [Step 1]
   - [Step 2]

**Links**

- Freelo: [url or blank]
- Figma: [url or blank]
- Github: [url or blank]
```

### Bug task template structure

```
**Description**

[Short plain English description of the bug]

*Additional requirement (include only if applicable):*
[text]

**Problem**

- [What is going wrong]

**Context**

- [Why it happens / background]

**Expected Behavior**

- [What should happen instead]

**Links**

- Freelo: [url or blank]
- Figma: [url or blank]
- Github: [url or blank]
```

## Step 6: Show the draft and wait for approval

Present the full draft description to the user in the conversation. **Do not touch ClickUp until the user explicitly approves.** This is non-negotiable — always show first, update after confirmation.

If the user requests changes, apply them and show the revised draft again before updating.

## Step 7: Update or create the task in ClickUp

Once the user approves:

- **Updating**: Use `clickup_update_task` with the approved description via `markdown_description`. Keep assignees, tags, priority, and status as confirmed in Step 2.
- **Creating**: Use `clickup_create_task` in the correct list via `markdown_description`. Add tags separately with `clickup_add_tag_to_task` if needed. Set priority and assignee as confirmed.

Always confirm with a link to the task when done.
