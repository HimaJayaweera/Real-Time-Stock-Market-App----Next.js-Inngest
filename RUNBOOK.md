# Runbook

Operational reference for running, building, and troubleshooting this project. Pair with `PROGRESS.md` (what's built) and `RESUME.md` (how to pick work back up).

## ⚠️ Before writing any code

Per `AGENTS.md`: this project pins **Next.js 16.2.0**, which is ahead of most models' training data and may include breaking API/convention changes. Before writing or editing framework-level code (routing, data fetching, caching, config), check the relevant guide under:

```
node_modules/next/dist/docs/
```

Docs are organized under `01-app/` (App Router — what this project uses) and `02-pages/` (Pages Router — not used here). Relevant starting points already confirmed present:
- `01-app/01-getting-started/` — installation, project structure, layouts/pages, data fetching, caching, error handling, metadata, route handlers.
- `01-app/03-api-reference/05-config/` — `next.config.ts` options.
- `01-app/02-guides/upgrading/` — codemods / migration notes if upgrading further.

Don't assume pre-16 App Router behavior (e.g. caching defaults, `fetch` semantics, config keys) still holds — verify against the local docs first.

## Prerequisites

- Node.js version compatible with Next.js 16 / React 19 (no `.nvmrc` or `engines` field is pinned in `package.json` — check `node_modules/next/package.json` `engines` field if unsure).
- npm (repo has a committed `package-lock.json`; use `npm`, not `yarn`/`pnpm`, to stay consistent with the lockfile).

## Setup

```bash
npm install
```

## Common commands

| Command | What it does |
|---|---|
| `npm run dev` | Start the Next.js dev server (default: http://localhost:3000). |
| `npm run build` | Production build. |
| `npm run start` | Serve the production build (run `build` first). |
| `npm run lint` | Run ESLint (flat config, `eslint-config-next` core-web-vitals + typescript). |

There is currently no test command configured — no test runner is present in `package.json`.

## Adding shadcn components

`components.json` is configured (style `radix-nova`, base color `neutral`, RSC on, icons via `lucide-react`). Use the shadcn CLI to add components rather than hand-rolling them, so they land in `components/ui/` with the project's conventions:

```bash
npx shadcn add <component>
```

Note: `components/ui/button.tsx` is currently deleted in the working tree — re-add it via the CLI if/when it's needed again rather than restoring the old file blindly, in case the component set has moved on.

## Styling notes

- Tailwind v4 is configured via `@import 'tailwindcss'` in `app/globals.css` (no `tailwind.config.*` — v4 uses CSS-based config via `@theme`). Don't go looking for a JS/TS Tailwind config file; it doesn't exist in this setup.
- Dark mode is forced on (`<html className="dark">` in `app/layout.tsx`), not toggled by a `prefers-color-scheme` or user setting. If a light mode / theme toggle is ever added, this is the place to change.
- Custom brand color tokens (`--color-dark-*`, `--color-purple-*`, `--color-green-*`, `--color-red-*`, `--color-yellow-*`) are already defined in `app/globals.css` — reuse these instead of introducing ad hoc hex values, especially for price-up/price-down/alert styling.

## Known gotchas

- `components/Header.tsx` currently references `<Link>` without importing it from `next/link` — dev server / build will error on any route that renders it (it's rendered globally from `app/layout.tsx`). See `PROGRESS.md` → Known issues.
- No environment variables are wired up yet. When a data provider (stock price API, auth, etc.) is added, create a `.env.example` documenting required keys — `.env*` is already gitignored.

## Deployment

No deployment target is configured yet beyond the default `create-next-app` README pointer to Vercel. Revisit this section once a hosting decision is made.
