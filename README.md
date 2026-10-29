# agent-playbook

How I build TypeScript and React projects with coding agents, packaged as a Claude Code plugin so it is one install away on any machine.

The idea in one line: **I decide and review; the agent drafts and implements.** Every decision is written down before code exists, the rules that matter are enforced by lint and a pre-commit gate, and implementation runs ticket by ticket in fresh agent sessions, each ending in a pull request I review.

## Install

```
/plugin marketplace add SpitzN/agent-playbook
/plugin install agent-playbook@agent-playbook
```

Update after pushing changes: `/plugin marketplace update agent-playbook`.

The flow leans on these plugins:

```
/plugin marketplace add mattpocock/skills
/plugin install mattpocock-skills@mattpocock
/plugin install superpowers@claude-plugins-official
/plugin install context7@claude-plugins-official
/plugin marketplace add pbakaus/impeccable
/plugin install impeccable@impeccable
```

## Skills

| Skill                          | What it does                                                                                   |
| ------------------------------ | ---------------------------------------------------------------------------------------------- |
| `/agent-playbook:new-project`  | The whole flow, phase by phase. Finds where a project stands and resumes from there.           |
| `/agent-playbook:ralph`        | The ticket loop: `dry-run`, `afk <n>`, `status`, `stop`. One fresh session, branch and pull request per ticket. |
| `/agent-playbook:lesson`       | Turns something that surprised me into a rule, written into this repo and pushed.              |

## The flow

| Phase            | Output                                                        | With                                              |
| ---------------- | ------------------------------------------------------------- | ------------------------------------------------- |
| 1. Product       | `docs/product.md`, `GLOSSARY.md`                              | `brainstorming`, `grill-with-docs`                |
| 2. Decisions     | `docs/adr/`                                                   | ADR per choice with a real alternative            |
| 3. Wireframes    | `docs/wireframes.md`                                          | ASCII layouts, iterated until settled             |
| 4. Architecture  | `docs/architecture.md`, with a "where state lives" table      |                                                   |
| 5. Guardrails    | `CLAUDE.md`, `.claude/rules/`                                 | templates in this repo                            |
| 6. Tickets       | `.scratch/<feature>/issues/NN-*.md`                           | `to-spec`, `to-tickets`                           |
| 7. Setup         | the stack, the lint config, `pnpm check` and the hook         | [setup.md](skills/new-project/setup.md)           |
| 8. The loop      | one pull request per ticket, reviewed and merged              | `ralph`, `code-review`                            |
| 9. Visual design | theme tokens, `DESIGN.md`                                     | `impeccable`                                      |
| 10. README       | run it, what was built, trade-offs, what next, how it was built |                                                 |

The full steps, with when each phase is done, are in [skills/new-project/SKILL.md](skills/new-project/SKILL.md).

## Where things are

| Path                                                             | What                                                              |
| ---------------------------------------------------------------- | ----------------------------------------------------------------- |
| [LESSONS.md](LESSONS.md)                                         | Rules for working with agents, and why each holds                 |
| [docs/stack.md](docs/stack.md)                                   | The default stack, its lint rules, and the traps found setting it up |
| [skills/new-project/templates/](skills/new-project/templates/)   | Files a new project starts from: `CLAUDE.md`, rules, configs, tracker docs |
| [skills/ralph/](skills/ralph/)                                   | The loop script, its session prompt and an offline self-test      |
