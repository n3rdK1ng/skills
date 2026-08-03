# skills

My custom [agent skills](https://skills.sh/) for Claude Code and other
skills-compatible agents, plus links to the community skills and plugins I use.

## Install

```sh
npx skills add n3rdK1ng/skills
```

Or clone and symlink individual skills into `~/.claude/skills/`:

```sh
git clone https://github.com/n3rdK1ng/skills.git
cd skills
ln -s "$(pwd)/skills/questions" ~/.claude/skills/questions
```

## Skills

| Skill | Description |
| --- | --- |
| [`clickup-task`](skills/clickup-task) | Create or update ClickUp tasks from Freelo tasks (or from scratch) with a strict format and approval workflow. |
| [`domain-model`](skills/domain-model) | Create or maintain a project's CONTEXT.md domain glossary and docs/adr/ decision records. |
| [`improve-codebase-architecture`](skills/improve-codebase-architecture) | Find deepening opportunities in a codebase, informed by the domain language in CONTEXT.md and the decisions in docs/adr/. Pairs with [`domain-model`](skills/domain-model), which it links into. |
| [`questions`](skills/questions) | Batch pending ambiguities into structured AskUserQuestion calls instead of plain-text questions. |

> **Note on `clickup-task`:** workspace-specific values (Notion template page
> IDs, teamspace names) live in a gitignored `LOCAL.md` next to the skill.
> Without it, the skill falls back to asking for your own values — see its
> Step 3. The file looks like this:
>
> ```markdown
> ## Notion template page IDs
>
> - Normal task: `<your-notion-page-id>`
> - Bug task: `<your-notion-page-id>`
>
> ## Teamspace / List locations
>
> - <your-list-name>
> ```

## Community skills I use

Installed via the [Skills CLI](https://github.com/vercel-labs/skills)
(`npx skills`), browsable at [skills.sh](https://skills.sh/):

- [agent-browser](https://github.com/vercel-labs/agent-browser) — browser automation CLI for agents
- [electron](https://github.com/vercel-labs/agent-browser) — Electron desktop app automation via agent-browser
- [dogfood](https://github.com/vercel-labs/agent-browser) — systematic exploratory QA of web apps
- [find-skills](https://github.com/vercel-labs/skills) — discover and install skills from the ecosystem

## Plugins & marketplaces I use

Managed by Claude Code's plugin system:

- [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official) — superpowers, code-review, feature-dev, pr-review-toolkit, commit-commands, skill-creator, serena, figma, sentry, context7, playwright, frontend-design, and more
- [cloudflare/skills](https://github.com/cloudflare/skills) — Cloudflare platform skills
- [expo/skills](https://github.com/expo/skills) — Expo / React Native skills
- GitKraken hooks (local directory marketplace)

## License

[MIT](LICENSE)
