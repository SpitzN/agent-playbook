---
name: new-project
description: Take a TypeScript and React project from an idea to merged pull requests, the owner's way - documents first, guardrails in lint, tickets run by the ralph loop.
disable-model-invocation: true
argument-hint: "[what the project is, or the phase to resume]"
---

The owner asked for: `$ARGUMENTS`

The owner decides and reviews; the agent drafts and implements. Every decision is written down before code exists, so each later session works from **documents**, never from this conversation. A phase ends when its completion criterion holds and the owner has read its output. One phase per sitting is normal.

Files to copy into the project are in `${CLAUDE_SKILL_DIR}/templates/`. The stack, its reasons and its traps are in `${CLAUDE_SKILL_DIR}/../../docs/stack.md`.

## 0. Find the starting phase

List which of these exist in the project: `docs/product.md`, `GLOSSARY.md`, `docs/adr/`, `docs/wireframes.md`, `docs/architecture.md`, `CLAUDE.md`, `.claude/rules/`, `.scratch/*/issues/`, `package.json` with a `check` script, merged ticket pull requests, `DESIGN.md`, a README with a "What would come next" section. Start at the first phase whose output is missing, and confirm it with the owner.

**Done when** the owner has agreed on the phase.

## 1. Product

Run `/superpowers:brainstorming` to shape the idea, then `/mattpocock-skills:grill-with-docs`: questions in rounds until no branch of the design is left assumed. Write:

- `docs/product.md`: what this is, the core requirements as a numbered list, scope (in this version and not in it), then every screen's behaviour with its edge cases, empty states and accessibility.
- `GLOSSARY.md`: each domain term with its definition and the synonyms to avoid.

**Done when** every screen has written behaviour for its empty, error and overflow cases, and every noun the UI shows is in the glossary.

## 2. Decisions

Each choice with a real alternative becomes an ADR in `docs/adr/NNNN-<slug>.md` as it is made, through `/mattpocock-skills:domain-modeling`: the decision as a fact, considered options with the reason each lost, consequences. Typical first ones: where server state lives (or why there is none), the table, the form, the component library, the layout, supported screen sizes.

**Done when** no open question in `docs/product.md` would change the code layout. The repository still holds only documents.

## 3. Wireframes

Draft `docs/wireframes.md`: an ASCII layout per screen, then what sits in each region and every state a user can see. Iterate with the owner until the layout of every screen is settled. Layout only: colour and type wait for phase 9.

**Done when** the owner has approved every screen.

## 4. Architecture

Write `docs/architecture.md`:

- domain types as Zod schemas, with the rules that follow from the model;
- the stack table, each row linking its ADR;
- **where state lives**: a table of every piece of state, its home, who would own it in a real product (server, client, derived), and why;
- routes, code layout, units with what each depends on, and where Zod parses (every boundary where data enters).

**Done when** every piece of state in `docs/product.md` has a row in the state table.

## 5. Guardrails

Run `/mattpocock-skills:setup-matt-pocock-skills` with the local markdown tracker; it writes `docs/agents/`. Then copy and adapt from `templates/`: `CLAUDE.md` and `.claude/rules/` (code-design, structure, react, ui). Fill the CLAUDE.md placeholders and prune what this project does not use.

A rule lint can check goes into lint (phase 7); `.claude/rules/` keeps only what lint cannot check. Each rule states the target behaviour.

**Done when** `CLAUDE.md` points at every document from phases 1 to 4 with the condition for reading it.

## 6. Spec and tickets

Run `/mattpocock-skills:to-spec`, then `/mattpocock-skills:to-tickets`. Tickets land as `.scratch/<feature>/issues/NN-<slug>.md`. Add the loop's statuses to `docs/agents/issue-tracker.md` (see `/agent-playbook:ralph`, What the loop expects). Each ticket is a thin end-to-end slice: what to build, the documents to read first, the seams to test at, `- [ ]` acceptance boxes, `**Blocked by:**` and `**Status:** ready-for-agent`. The last two tickets are a visual design pass and the README.

**Done when** the owner has read every ticket and each one is small enough for one session.

## 7. Setup and the gate

Follow `${CLAUDE_SKILL_DIR}/setup.md`, together with the owner. Install the whole stack and generate every UI component the tickets need up front: an unattended session cannot answer a CLI prompt, and a dependency it wants is a question for the owner.

**Done when** `pnpm check` is green, a commit with a failing check is refused, and every lint rule has a probe file that fails it.

## 8. The loop

Run `/agent-playbook:ralph dry-run` and check the order with the owner. Run the first two or three tickets hands-on (`ralph.sh once`), then `afk <n>` for the rest.

The owner reviews every pull request before merging, bottom of the stack first. A review finding that changes a rule goes back into the documents or `.claude/rules/` in the same fix, so later tickets follow it.

**Done when** every ticket before the design pass is merged.

## 9. Visual design

The design ticket runs `/impeccable:impeccable` through the theme tokens in `src/index.css`, with components left as they are. It writes `DESIGN.md`.

**Done when** both themes pass a check of every screen, and `DESIGN.md` describes the tokens in use.

## 10. README

Sections: Run it (one command from a fresh clone), What was built (a route table and a short tour), Decisions and trade-offs (the state table, the ADR list), What would come next, How it was built with AI (these phases, in the owner's words), Where things are.

**Done when** a fresh clone runs with the documented command and every link in the README resolves.

## Afterwards

Run `/agent-playbook:lesson` for anything that surprised the owner, so the next project starts from it.
