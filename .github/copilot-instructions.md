# Copilot Instructions for `spark-template`

## Project snapshot
- This repo is a minimal Spark + React template (not a feature app yet); `src/App.tsx` is intentionally empty.
- App bootstrap lives in `src/main.tsx`: React root + `ErrorBoundary` + Spark runtime import (`@github/spark/spark`).
- Stack: Vite 7, React 19, TypeScript, Tailwind CSS v4.

## Non-obvious constraints
- In `vite.config.ts`, keep both Spark plugins enabled: `createIconImportProxy()` and `sparkPlugin()`.
- Removing either plugin can break Spark/runtime behavior.
- Use the `@` path alias for app imports (`@/*` -> `src/*`) per `tsconfig.json` and `vite.config.ts`.

## Key workflows
- Install deps: `npm install`
- Dev server: `npm run dev`
- If port 5000 is stuck: `npm run kill`
- Production build: `npm run build`
- Lint: `npm run lint`

## Code structure patterns
- `src/components/ui/*` contains shadcn/Radix-style primitives (typed wrappers with `data-slot`).
- Reuse `cn(...)` from `src/lib/utils.ts` for class merging (`clsx` + `tailwind-merge`).
- Prefer composing existing primitives before adding new low-level UI wrappers.
- Form pattern is centralized in `src/components/ui/form.tsx` (`Form`, `FormField`, `FormItem`, `FormControl`, `FormMessage`).
- Chart pattern is centralized in `src/components/ui/chart.tsx` (`ChartContainer`, themed CSS variables, tooltip/legend helpers).

## Styling and theming
- Tailwind entrypoint is `src/main.css` (`@import 'tailwindcss'`, `@config '../tailwind.config.js'`).
- Theme tokens come from CSS variables (`src/styles/theme.css`, `src/main.css`) and optional overrides in `theme.json` via `tailwind.config.js`.
- Use token utilities (`bg-background`, `text-foreground`, `border-border`) instead of hardcoded colors.

## Error handling behavior
- `src/ErrorFallback.tsx` intentionally rethrows errors in development (`if (import.meta.env.DEV) throw error;`).
- Do not change this behavior unless explicitly requested; it preserves Spark dev diagnostics.

## Do / Don’t for agents
- Do keep changes template-friendly and minimal.
- Do use `@/` imports and existing UI primitives before creating new abstractions.
- Do preserve `data-slot` patterns in UI wrappers for consistency.
- Don’t remove Spark Vite plugins or alter bootstrap order in `src/main.tsx`.
- Don’t add a root test framework unless explicitly asked (root app has no test script).
- Don’t hardcode theme colors when tokenized classes/variables already exist.

## Workspace notes
- Monorepo workspace is enabled (`packages/*`), but the app mostly consumes published `@github/spark` APIs.
- Internal package code exists under `packages/spark-tools`, but typical app changes belong in `src/*`.
