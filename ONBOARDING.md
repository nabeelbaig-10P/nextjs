# Onboarding: Next.js Crash Course App

A small Next.js 13 App Router demo/learning project. It showcases App Router
conventions (server components, streaming, route handlers, metadata API)
rather than being a production product — good for getting comfortable with
the App Router before working in bigger Next.js codebases.

## Getting started

```bash
npm install
npm run dev
```

Open http://localhost:3000. That's it — no environment variables, database,
or external services required to run locally.

Other scripts:

```bash
npm run build   # production build (outputs to ./build, not the default .next)
npm run start   # serve the production build
npm run lint    # next lint
```

There's no test suite in this repo.

## What's in here

- `app/` — everything, using the App Router. A folder is a route segment;
  `page.jsx` renders it, `layout.jsx` wraps it, `loading.jsx` is the
  Suspense fallback shown while a segment's data loads.
- `app/componets/` — shared components (yes, misspelled — it's the real
  directory name, not a typo to fix).
- `app/api/**/route.js` — API route handlers (`GET`/`POST` exports).
- `app/code/repos/` — pulls live data from the public GitHub API
  (unauthenticated, so subject to GitHub's rate limits) to demo
  server-side fetching + streaming with nested `<Suspense>` boundaries.
- Home page (`app/page.jsx`) demos the other pattern: a client component
  (`'use client'`) that fetches from this app's own `/api/courses` routes
  in a `useEffect`.
- `app/api/courses/data.json` is an in-memory course list. `POST` to
  `/api/courses` appends to it in memory only — nothing is persisted, so
  it resets on every server restart.

See `CLAUDE.md` for more implementation-level detail (written for AI
assistants working in this repo, but useful background for humans too).

## CI

The `Jenkinsfile` doesn't build or test this app — it runs an internal
AI PR-review bot (`10pdocker/ai-pr-bot`) against pull requests.

## Good first tasks

- Fix the `app/componets` → `app/components` spelling (repo-wide rename;
  currently left as-is, so check with the team before doing this).
- Persist `POST /api/courses` writes instead of only mutating the
  in-memory array.
- Add a test setup — none exists yet.
