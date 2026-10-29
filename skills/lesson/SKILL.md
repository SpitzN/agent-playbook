---
name: lesson
description: Record a lesson from this session in the agent-playbook repository, so the next project starts from it.
disable-model-invocation: true
argument-hint: "[what surprised you]"
---

The owner asked for: `$ARGUMENTS`

A lesson is one rule that would have changed how this session went, with the reason it holds. It is written as a **pattern**, portable to any project.

## 1. Name the lesson

From the session and `$ARGUMENTS`, state what went differently from what the owner expected, and the one rule that would have prevented it or made it repeatable. When the session holds several, list them and let the owner pick.

**Done when** the owner agrees on the rule in one sentence.

## 2. Choose its home

| The lesson is about                                    | Its home                                                             |
| ------------------------------------------------------ | -------------------------------------------------------------------- |
| How to work with agents                                | `LESSONS.md`: a `##` title, a `**Why:**` line, a `**Rule:**` line    |
| A tool, library or config trap                         | `docs/stack.md`, under Traps                                         |
| A rule every new project should carry                  | the matching file under `skills/new-project/templates/`              |
| A step of the flow                                     | `skills/new-project/SKILL.md` or `skills/new-project/setup.md`       |
| The ticket loop                                        | `skills/ralph/`, then run `selftest.sh` until it passes              |

An existing entry that covers the lesson is sharpened in place.

## 3. Write it generic

Write it in terms any project shares: tools, libraries, file kinds, agent behaviour. The text carries no name of an organisation, client, product, person or domain from the session's project. The repository is public.

## 4. Commit and push

Find the playbook's checkout: `$AGENT_PLAYBOOK_DIR`, else `~/Projects/agent-playbook`, else `gh repo clone SpitzN/agent-playbook` into a temporary directory. Pull first. Show the owner the diff; on their approval, commit with an imperative subject and push to `main`. When a skill changed, bump `version` in `.claude-plugin/plugin.json`.

**Done when** the commit is on `origin/main`. Remind the owner that installed copies update with `/plugin marketplace update agent-playbook`.
