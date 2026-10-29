# Stack

The default stack for a TypeScript and React frontend, why each piece is there, and the traps found while setting it up. Verified in October 2026; check the current majors before relying on a version below.

## Defaults

| Concern          | Choice                                                                 | Why                                                                                   |
| ---------------- | ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| Build            | Vite, React 19 with the React Compiler                                 | The compiler makes memoization automatic, so code stays plain                         |
| Language         | TypeScript, `strict` + `noUncheckedIndexedAccess` + `exactOptionalPropertyTypes` | Types by construction; no assertions                                         |
| Package manager  | pnpm                                                                   | Strict dependency resolution; build scripts allowed one by one                        |
| Routing          | React Router                                                           | Address-driven state (filters, sort, open drawer) is linkable and survives a reload    |
| Client state     | Zustand, with `persist` when it must survive a reload                  | Small, hook-shaped, no provider                                                       |
| Server state     | TanStack Query, when there is a real API                               | Caching, pending and error states, mutations. Never wrapped around in-memory data     |
| Schemas          | Zod 4; every domain type is `z.infer`-ed                               | One source per fact; parses at every boundary                                         |
| Forms            | React Hook Form with the Zod resolver                                  | Uncontrolled inputs, schema validation                                                |
| Tables           | TanStack Table, given controlled state                                 | The table owns no state; sort and filters live in the address                         |
| Components       | shadcn/ui, Base UI variants only                                       | Generated source the project owns; a missing look is a `cva` variant                  |
| Styling          | Tailwind v4, theme tokens in `index.css`                               | Every colour is a token, light and dark                                               |
| Theme            | `next-themes`                                                          | Follows the system setting, remembers the choice. No Next.js dependency               |
| Tests            | Vitest, beside the code                                                | Unit tests on pure functions and seams                                                |
| Checks           | `tsc -b`, ESLint, Prettier, husky pre-commit running `pnpm check`      | One gate, run identically by a person, an agent and the hook                          |

## Lint plugins

A lean set; each rule must be defensible. typescript-eslint (`strictTypeChecked` + `stylisticTypeChecked`), `eslint-plugin-react-hooks` (React Compiler rules), `eslint-plugin-react-refresh`, `eslint-plugin-jsx-a11y` (strict), `eslint-plugin-better-tailwindcss`, `@shadcn/lint`, `eslint-plugin-boundaries` with `eslint-import-resolver-typescript`, and `eslint-config-prettier` last.

The rules that carry the most weight:

- `linterOptions.noInlineConfig: true`: disable comments are inert, so a failing rule is fixed in the code.
- `consistent-type-assertions: never`, `ban-ts-comment`, no `any`, no non-null assertions.
- `complexity` 10, `max-depth` 2, `max-params` 3, `max-nested-callbacks` 2, `no-else-return`, `no-nested-ternary`.
- No re-exports, no export lists, no default exports, no `../` imports.
- Boundaries: layers and feature slices, `default: "disallow"`.
- Tailwind: no arbitrary values, no raw palette colours, no text above 24px.

## Traps

- **`tsc --noEmit` checks nothing in Vite's template.** The root `tsconfig.json` lists only references, so `--noEmit` exits 0 on a type error. Use `tsc -b`.
- **TypeScript 7 vs typescript-eslint.** When npm's `latest` TypeScript is ahead of what typescript-eslint supports, pin TypeScript to the newest supported minor (`~6.0.x` in October 2026), so one compiler both type-checks and lints.
- **`vitest run` exits 1 with no test files.** Set `passWithNoTests` so the gate passes before the first tested seam lands.
- **The React Compiler and stable objects.** A compiled child handed an object whose contents change internally (TanStack Table's `table` and `row`, React Hook Form's `form`) keeps showing stale data. Call their methods in the owning component and pass plain values down; children read form state through `useWatch`, `useFormState` or `Controller`. `watch()` makes the compiler skip the component.
- **typescript-eslint presets reject ordinary React code** in three places: a promise-returning handler in a JSX attribute (`no-misused-promises` → `checksVoidReturn.attributes: false`), a number in a template literal (`restrict-template-expressions` → `allowNumber`), an arrow returning a `void` call (`no-confusing-void-expression` → `ignoreArrowShorthand`).
- **`eslint-plugin-boundaries` passes an `@/` import it cannot resolve.** By default it counts as an external package and skips every check. Add `eslint-import-resolver-typescript` and set `flag-as-external: { unresolvableAlias: false }`.
- **Boundaries skips two kinds of import by default**: one inside a single element folder, and one of a file that belongs to no element. Set `checkInternals` and `checkUnknownLocals`.
- **In boundaries v7 an element is a folder.** File patterns match nothing; classify single files (`main.tsx`, tests, CSS) with `boundaries/files` categories.
- **shadcn-generated files fail strict project options** (`noUnusedLocals`, `exactOptionalPropertyTypes`). Keep `src/components/ui/` out of ESLint and compile it under its own `tsconfig.ui.json` with `strict` alone, referenced by the app config.
- **`shadcn add` stops at a prompt** when a file it writes already exists. A component that depends on an existing one needs `--overwrite`, which also drops project variants in that file: add them again.
- **`@shadcn/lint` cannot read a `cva` result or a class picked by key** on a primitive's `className`. The look is a variant in the primitive's own `cva`.
- **The arbitrary-value ban also rejects structural values** such as `grid-cols-[auto_1fr]`. Add a theme token or utility instead.
- **Generated `src/lib/utils.ts` only re-exports `cn`**, which the re-export ban refuses. Delete it and import from the `cn` package; `shadcn add` does not recreate it.
- **pnpm refuses dependency build scripts** unless listed under `allowBuilds` in `pnpm-workspace.yaml`. Allow esbuild; refuse `unrs-resolver`, whose native binding arrives prebuilt.
- **`pnpm-lock.yaml` fails the format check** unless it is in `.prettierignore`.
- **Prettier reads `.gitignore`; ESLint and Vitest do not.** Ignore `.scratch/` and `.claude/worktrees/` in all three, or agents' worktrees get linted and tested.
