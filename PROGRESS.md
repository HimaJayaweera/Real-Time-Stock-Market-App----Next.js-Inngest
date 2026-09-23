# Progress

Snapshot date: 2026-09-11. Update this file whenever a meaningful chunk of work lands — don't let it drift more than a session or two out of date.

## Tech stack (as installed)

- **Framework:** Next.js 16.2.0, App Router (`app/`) — see `RUNBOOK.md` for the note on this being a pre-release/breaking-changes version.
- **UI runtime:** React 19.2.4 / React DOM 19.2.4, TypeScript 5 (strict mode on).
- **Styling:** Tailwind CSS v4 via `@tailwindcss/postcss`, plus `tw-animate-css`. Theme tokens and a custom "brand" palette (dark-900/700/500/400, purple-600/100, green-500/400/300, red-500, yellow-500) live in `app/globals.css` — looks intended for a dark-first stock/trading UI.
- **Components:** shadcn/ui, configured via `components.json` with style `radix-nova`, base color `neutral`, RSC on, icon library `lucide-react`. Uses the `radix-ui` umbrella package rather than individual `@radix-ui/react-*` packages.
- **Utilities:** `class-variance-authority`, `clsx`, `tailwind-merge` (combined in `lib/utils.ts` as `cn()`).
- **Lint:** ESLint 9 flat config (`eslint.config.mjs`), `eslint-config-next` core-web-vitals + typescript rule sets.
- **Fonts:** Geist Sans / Geist Mono via `next/font/google`, exposed as CSS variables in `app/layout.tsx`.

## Completed

- Project scaffolded from `create-next-app`.
- Dark mode forced globally (`<html className="dark">` in `app/layout.tsx`).
- shadcn/Tailwind v4 theme wired up in `app/globals.css` with a custom brand palette (suggests colors for price up/down, alerts, etc. are already planned: green/red/yellow tokens).
- `lib/utils.ts` — standard shadcn `cn()` helper.
- `components/Header.tsx` — skeleton only (see Known issues below).

## In progress / uncommitted (working tree as of this snapshot)

Per `git status` at time of writing:
- `app/globals.css`, `app/layout.tsx`, `app/page.tsx` — modified, not committed.
- `components/ui/button.tsx` — deleted in the working tree (still present in the last commit). Likely being replaced or regenerated via `npx shadcn add button`.
- `components/Header.tsx` — new, untracked.

Only one commit exists on `master` (`2e43a24 feat: initial commit`), so essentially everything above is still pre-first-real-commit scaffolding.

## Not started

- No stock-data fetching, API routes, or route handlers yet (`app/api/` doesn't exist).
- No actual page content — `app/page.tsx` currently renders an empty centered `<div>`.
- No routing beyond the root `/` page.
- No state management, data layer, or external API integration decided yet.
- No tests configured (no test runner in `package.json`).
- No environment variables in use yet (`.env*` is gitignored, no `.env.example` present).

## Known issues

- `app/page.tsx` has no real content — placeholder only.
- `components/Header.tsx`'s `<Link href="/">` still has no visible content (empty tag) — not a build error, but worth filling in once there's a brand/logo to link.

### Resolved

- ~~`components/Header.tsx` missing `Link` import~~ — fixed 2026-09-11 (added `import Link from 'next/link'`).
