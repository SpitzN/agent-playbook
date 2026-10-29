# Writing rules

How to write the rules an agent follows: where each one lives, how to word it, and how to keep the set small and true. The full reference is `/mattpocock-skills:writing-for-agents`; this is the working summary.

## Where a rule lives

Pick the first home that can hold it.

| Home | Holds | Why first |
| --- | --- | --- |
| **The compiler and lint** | Anything a tool can check: types, imports, layers, complexity, class names, accessibility | Enforced on every commit, whether or not the agent read anything. `noInlineConfig` makes it unskippable |
| **`.claude/rules/<topic>.md`** with `paths:` | Conventions a tool cannot check, for one kind of file | Loaded only when the agent works on matching files |
| **`CLAUDE.md`** | Working agreements (the gate, dependencies, who adds components) and pointers to every document | Loaded every turn, so every line costs |
| **An ADR** | A decision with a real alternative, and its reason | The reason keeps the decision from being re-argued |
| **`docs/*.md`** | Product behaviour, layout, architecture, tooling | Reached through a pointer in `CLAUDE.md` |

A rule that exists in two homes drifts. Lint rules are listed once, in `docs/tooling.md`, and `.claude/rules/` says "lint enforces X" without repeating it.

## Path-scoped rule files

```markdown
---
paths:
  - "src/**/*.tsx"
---

# React

- Vary a component through `children` and composed parts before adding a prop.
```

One file per topic (code design, structure, React, UI), scoped to the files it governs. Keep each file to one screen.

## Wording

- **State the target behaviour.** "Derived values are computed during render" lands better than "don't store derived state". A prohibition puts the forbidden thing in the agent's attention; use one only for a hard guardrail, paired with what to do instead.
- **One rule, one line, one reason when it is not obvious.** "Features never import each other: a route composes them."
- **Concrete over abstract.** Name the file, the folder, the function, the command.
- **Use the glossary's terms.** The same word in the rule, the code and the prompt helps the agent connect them.
- **Leave out what the agent does by default.** A rule earns its place by changing behaviour. "Write clean code" changes nothing.
- **Do not restate config.** `package.json`, `tsconfig.json` and the folder layout are their own source of truth. Write down what they cannot say: the reason, the convention, the trap.

## Pointers in `CLAUDE.md`

Each pointer names the document and the condition for reading it:

```markdown
- `docs/wireframes.md`: how each screen looks. Read before building or changing a screen.
```

The condition does the work. "See docs/wireframes.md" gets skipped; "read before building a screen" gets followed.

## Keeping rules true

- **Write back after review.** When a review changes a rule, the same fix updates the rule's home, so the next session follows it.
- **Probe new lint rules.** A rule that must fail gets a bad file that fails it, before you trust it.
- **Prune.** When a rule no longer matches the code, fix one of them the same day. Stale rules teach the agent to ignore rules.
- **Watch for repeats.** The second time you write the same review comment, it becomes a rule (prompt: "Turn a review comment into a rule" in [prompts.md](prompts.md)).
