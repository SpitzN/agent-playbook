# Setup and the gate

Phase 7 of `new-project`. Templates are in `templates/` beside this file; the stack and its traps are in `docs/stack.md` at the playbook's root. Versions move: before each install, check the current major and its peer ranges (context7 or the package's changelog), and record what you verified in `docs/tooling.md`.

Each step is its own commit, in the repo's commit style.

## Steps

1. **Scaffold.** Vite's `react-ts` template with pnpm. Remove the demo content. Add the `@/` alias to `vite.config.ts` and the tsconfig files.
2. **Configs.** Copy from `templates/`: `tsconfig.json`, `tsconfig.app.json`, `tsconfig.ui.json`, `tsconfig.node.json`, `vite.config.ts`, `eslint.config.js`, `.prettierrc.json`, `.prettierignore`, `pnpm-workspace.yaml`, and `gitignore` as `.gitignore`. Adapt the boundaries elements in `eslint.config.js` to the code layout in `docs/architecture.md`.
3. **Scripts.** In `package.json`:

   ```json
   "typecheck": "tsc -b",
   "lint": "eslint . --max-warnings 0",
   "format": "prettier --write .",
   "format:check": "prettier --check .",
   "test": "vitest run",
   "check": "pnpm typecheck && pnpm lint && pnpm format:check && pnpm test"
   ```

4. **The hook.** husky, with `templates/.husky/pre-commit` (`pnpm check`). No lint-staged: the hook runs exactly what a person or an agent runs by hand.
5. **Probe the gate.** For every lint rule that must fail, write a small "bad" file and confirm the rule rejects it; write "good" files that must pass. Delete the probes after. Record the run, its date and any rule that behaved unexpectedly in `docs/tooling.md`.
6. **The whole stack, up front.** Install every package in the stack table of `docs/architecture.md` now, each one owner-approved.
7. **UI components, up front.** Run shadcn init with the Base UI variant, pointing the `hooks` alias at `@/components/ui/hooks` so everything the CLI writes is in one folder. Add every component the tickets name, then `pnpm format`. Delete the generated `src/lib/utils.ts` and import `cn` from the `cn` package.
8. **Write `docs/tooling.md`.** The gate table, a decisions table (each config choice and why), what lint enforces, what it cannot enforce (those stay in `.claude/rules/`), and the traps found while probing.

## Done when

- `pnpm check` is green on a clean clone after `pnpm install`.
- A commit with a type error, a lint error or a formatting error is refused by the hook.
- Every rule in `eslint.config.js` has a probe that failed it.
- `.scratch/` and `.claude/worktrees/` are ignored by git, ESLint and Vitest.
