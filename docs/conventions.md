# Conventions

What every project follows by default. The detailed rules an agent reads are the templates in [skills/new-project/templates/.claude/rules/](../skills/new-project/templates/.claude/rules/); the stack and its lint setup are in [stack.md](stack.md). A project departs from a convention through an ADR.

## Documents before code

- Product behaviour, glossary, ADRs, wireframes and architecture exist before the first ticket.
- Domain terms come from `GLOSSARY.md`, in code names, UI copy, commits and docs.
- A choice with a real alternative gets an ADR when it is made.

## TypeScript

- Strict, with `noUncheckedIndexedAccess` and `exactOptionalPropertyTypes`.
- Types come from construction: `z.infer` from Zod schemas, `satisfies`, discriminated unions. No assertions, no `any`, no `!`.
- Zod parses wherever data enters: the network, storage, the address bar, forms. Inside those boundaries, types are trusted.
- Return types are inferred.
- Named exports at the declaration; no default exports, no barrels, no re-exports.

## Code design

- Deep modules: a small interface over real work. A second responsibility is a reason to split; length is not.
- Build for a caller that exists. Extract on the third use.
- One source for each fact; everything else derives from it.
- Guard clauses and early returns; a boolean parameter that switches behaviour becomes two functions.
- A comment says what the code cannot: the promise to the caller, or why this path was chosen.

## Structure

- Layers: `app` → `features/<name>` → `domain` → shared `components`, `hooks`, `lib`. Lint enforces the direction.
- Features never import each other; a route composes them.
- Inside a feature, a file's folder says what it is: `components/`, `hooks/`, `stores/`, `schemas/`, `lib/`.
- Files in kebab-case; hooks are `use-<what>.ts`; tests sit beside the file they test.
- `@/` for imports outside the folder.

## State

Decide each piece's home and write it in the "where state lives" table in `docs/architecture.md`:

| Kind | Home |
| --- | --- |
| Server data | TanStack Query (or a static typed module while there is no API) |
| Shareable view state: filters, sort, open panel | The URL, Zod-parsed |
| Client state used across pages | A Zustand store |
| Form drafts | React Hook Form |
| Preferences | localStorage |
| Anything computable from the above | Computed during render, never stored |

## React

- The React Compiler memoizes; code stays free of `useMemo`, `useCallback` and `memo`.
- Effects only synchronise with something outside React.
- Compose small parts; vary through `children` and slots before props.
- Children receive plain values, not library objects that change internally.

## UI and accessibility

- shadcn/ui primitives, as generated; a missing look is a `cva` variant on the primitive.
- Every colour is a theme token for light and dark.
- Every interactive element has hover and focus states; every icon-only control has a tooltip and an accessible name; everything on hover is reachable by keyboard.
- Every list, table and panel has an empty state; overflowing text truncates with the full value reachable.
- One primary action per screen; destructive actions confirm.

## Testing

- Vitest, beside the code. Test pure functions and the seams a ticket names, through their public interface.
- Tests first for logic (`/mattpocock-skills:tdd`); a bug fix starts with a failing test.
- `pnpm check` (typecheck, lint, format, tests) is the definition of done.

## Git and pull requests

- One ticket, one branch (`feat/<feature>-<ticket>`), one pull request.
- Commit subjects are imperative and say what changed for the user or the code ("Keep long names inside their boxes"), with a `Ticket:` line when there is one.
- The pre-commit hook runs `pnpm check`; nothing skips it.
- Pull request bodies: Summary, Evidence (what was run and what it showed), Merge Danger.
- A new dependency needs the owner's approval, recorded in the pull request.
