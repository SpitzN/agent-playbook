# Lessons

What working with coding agents has taught me, one entry per lesson: the rule, and why it holds. Tool and library traps live in [docs/stack.md](docs/stack.md#traps). Add entries with `/agent-playbook:lesson`.

## Agents work from documents, not from the conversation

**Why:** a decision made in chat is gone by the next session, and the agent fills the gap with its own defaults.

**Rule:** every decision is written down (product doc, glossary, ADR, wireframes) before code exists, and `CLAUDE.md` points at each document with the condition for reading it.

## A rule in prose is a suggestion; a rule in lint is a rule

**Why:** a rule the agent is only told about drifts over a long run, and a disable comment is the fastest way past a failing rule.

**Rule:** whatever lint can check goes into lint, with `noInlineConfig` so disable comments are inert. `.claude/rules/` keeps only what lint cannot check, and `docs/tooling.md` lists which is which.

## One gate, run the same way by everyone

**Why:** without one command, "done" means different things to the agent and to me.

**Rule:** `pnpm check` is the definition of done, run by hand, by the agent, by the ticket loop and by the pre-commit hook.

## Unattended sessions cannot answer prompts

**Why:** a CLI that asks a question, or a dependency the agent wants, stalls a session nobody is watching.

**Rule:** install the whole stack and generate every UI component before the loop starts. A new dependency or an owner decision makes the ticket `needs-info` and stops the run.

## Thin tickets, fresh sessions

**Why:** a long session loses track of earlier decisions.

**Rule:** each ticket is a thin end-to-end slice with its acceptance boxes and the documents to read first, implemented in a fresh session on its own branch and pull request. A progress file carries what earlier tickets built.

## Prove the loop before leaving it alone

**Why:** the first tickets expose gaps in the prompt and the gate that would otherwise repeat on every ticket.

**Rule:** run the first two or three tickets hands-on, then let the loop run unattended. A ticket that fails twice stops the run.

## A review that changes a rule changes the documents

**Why:** otherwise the same review comment comes back on every later pull request.

**Rule:** when review changes a rule, the fix also updates the rule's home (an ADR, `.claude/rules/`, `docs/tooling.md`), so later tickets follow it.

## Probe the checks before trusting them

**Why:** some rules pass silently because of a resolver default or a type-checker setting (see the traps in docs/stack.md).

**Rule:** for every rule that must fail, write a bad file and watch it fail. Record the probe run and its date in `docs/tooling.md`.
