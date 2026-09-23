# Resume Protocol

Read this file first when picking up work on this project after a break (new session, new day, or handing off). It tells you where to look and in what order — it does not duplicate their content.

## Read order

1. **This file** — orientation and current focus.
2. `AGENTS.md` / `CLAUDE.md` — standing project instructions. Note: this repo pins a Next.js version ahead of typical training data; `AGENTS.md` requires checking `node_modules/next/dist/docs/` before writing framework-level code. `RUNBOOK.md` has the specifics.
3. `PROGRESS.md` — what's built, what's mid-flight, what's not started, and known bugs.
4. `RUNBOOK.md` — how to install, run, build, lint, and add shadcn components; styling conventions; gotchas.
5. `git status` / `git log` — ground-truth for what's actually changed since `PROGRESS.md` was last updated. Trust this over the snapshot if they disagree, and update `PROGRESS.md` accordingly.

## Current focus (as of 2026-09-14)

Working tree is clean — everything is committed (`01a3e86 Initial commit`, `f89729f changes to the folder structure were made`, `2e43a24 feat: initial commit`). The app is named **"Signalist"**, a stock-market dashboard, built on Next.js 16 / React 19 / TypeScript / Tailwind v4 / shadcn (Radix) UI, dark-themed.

`components/Header.tsx` is now fully wired: logo `<Image>` (fixed — statically imported from `@/assets/icons/logo.svg` with explicit `width={140} height={32}`, since a bare string `src` doesn't work for files outside `public/` and Next can't infer intrinsic size from it), `<NavItems />`, and `<UserDropdown />`.

- **`components/NavItems.tsx`** — client component, reads route links from `lib/constants.ts` (`NAV_ITEMS`: `/` Dashboard, `/search` Search, `/watchlist` Watchlist) and highlights the active one via `usePathname()`. It was originally `NavItems.jsx`; renamed to `.tsx` because `tsconfig.json`'s `include` only globs `**/*.ts`/`**/*.tsx` (not `.jsx`), so a `.jsx` file was invisible to the TS language service and never showed up in auto-import suggestions.
- **`components/UserDropdown.tsx`** — client component (`'use client'`), avatar + account dropdown (shadcn `DropdownMenu`, `Avatar`), `handleSignOut` currently just `router.push('/sign-in')` with no real auth call, and `user` is a hardcoded placeholder object (`{ name: 'John', email: 'abc@gmail.com' }`), not real session data. Had a bug where `DropdownMenuTrigger` used a `render={<Button .../>}` prop (Base UI/MUI convention) instead of Radix's `asChild` — fixed to `<DropdownMenuTrigger asChild><Button>...</Button></DropdownMenuTrigger>`.
- **`components/ui/`** now has `avatar.tsx`, `button.tsx`, `dropdown-menu.tsx` (shadcn-generated). Note: all three import `cn` from the **`cn` npm package** (a dependency, `"cn": "^0.3.0"`), not from `@/lib/utils` — even though `components.json` still declares `"utils": "@/lib/utils"` and `lib/utils.ts` still defines its own `cn()` via `clsx`+`tailwind-merge`. That local `cn()` is currently dead code. Not urgent, but worth resolving one way (repoint components.json / regenerate to use `@/lib/utils`, or delete the unused local `cn()`) before it causes confusion.

**⚠️ Still unresolved — flagged before, not yet fixed:**
1. **Duplicate root route.** `app/page.tsx` and `app/(root)/page.tsx` both resolve to `/`; `app/layout.tsx` and `app/(root)/layout.tsx` both render `<Header />`. This is a real conflict (Next.js should error building/serving `/`), not a nesting choice. The `(root)` route-group version looks like the intended pattern (its layout wraps children in a `container py-10` div) — the loose `app/page.tsx` / `app/layout.tsx` Header render looks like leftover scaffolding from before the route group was introduced.
2. **`/sign-in` route referenced but doesn't exist.** `UserDropdown`'s sign-out redirects there; no `app/sign-in/` (or `app/(auth)/sign-in/`) route has been created yet, so that link currently 404s.
3. **`/search` and `/watchlist` routes referenced in `NAV_ITEMS` but don't exist yet** — same situation, nav links will 404 until those pages are built.

Reasonable next steps, in rough order:
1. Resolve the duplicate `/` route between `app/page.tsx` and `app/(root)/`.
2. Decide on an auth approach and add the `/sign-in` route (and wire `UserDropdown`'s hardcoded `user` to real session data once auth exists).
3. Build out `/search` and `/watchlist` pages, or remove them from `NAV_ITEMS` until they're ready.
4. Reconcile the `cn` npm package vs. `@/lib/utils` `cn()` duplication.
5. Define the core stock-app data layer (price feed/API) — the `dashboard*.png` assets and brand color tokens (green/red for price direction) hint at the direction but no data fetching exists yet.

## Update protocol

- Update `PROGRESS.md` whenever a feature/chunk of work is completed, started, or abandoned — don't let "Completed" drift out of sync with `git log`.
- Update the "Current focus" section here at the end of a working session with where things were left and what the immediate next step is, so the next session doesn't have to re-derive it from scratch.
- Update `RUNBOOK.md` when commands, setup steps, env vars, or conventions change — it should always reflect how to actually run the project *today*, not how it used to run.
- If `AGENTS.md`/`CLAUDE.md` conventions change, re-read them fully rather than assuming this file's summary still holds — this file only points at them, it doesn't own their content.
