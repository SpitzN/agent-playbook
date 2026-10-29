# agent-playbook

A Claude Code plugin holding my workflow for building with agents: skills under `skills/`, the default stack in `docs/stack.md`, lessons in `LESSONS.md`. `README.md` is the map.

## Working agreements

- This repository is public. Everything in it is written as a portable pattern, in terms of tools, libraries and agent behaviour, with no name of an organisation, client, product, person or a specific project's domain.
- Skills and `CLAUDE.md` templates are documents an agent reads: write them with `/mattpocock-skills:writing-for-agents`.
- A lesson earns its place by changing behaviour. A rule an agent already follows by default is left out.
- After a change under `skills/ralph/`, `bash skills/ralph/selftest.sh` ends with `ALL CHECKS PASSED`.
- A change to a skill bumps `version` in `.claude-plugin/plugin.json`, so installed copies pick it up.
