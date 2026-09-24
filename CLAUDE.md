# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

A Next.js 13 "crash course" demo app (App Router / `app` directory, `experimental.appDir` enabled in `next.config.js`). It's a learning/reference project showcasing: App Router file conventions, React Server Components, streaming with `Suspense` and `loading.js`, Route Handlers (API routes), the Metadata API, and client-side data fetching.

## Commands

```bash
npm install      # install dependencies
npm run dev       # start dev server at http://localhost:3000
npm run build     # production build (output goes to ./build, not .next — see next.config.js)
npm run start     # run the production build
npm run lint      # next lint
```

There is no test suite or CI test stage in this repo.

## Architecture

- **Router**: everything lives under `app/`, using the Next.js 13 App Router. Folder = route segment; `page.jsx` renders the segment, `layout.jsx` wraps it, `loading.jsx` is the Suspense fallback.
- **Root layout** (`app/layout.jsx`) sets global metadata, loads the Poppins font via `next/font/google`, and wraps every page in `<Header />` + a `container` `<main>`.
- **Shared components** live in `app/componets/` (note: misspelled directory name — match it exactly when importing, don't "fix" it to `components`). Import shared components via the `@/*` path alias (configured in `jsconfig.json`, resolves to repo root) or relative paths, matching existing usage in each file.
- **Server vs. client components**: most page/data components (`Repo`, `RepoDirs`, `app/code/repos/page.jsx`) are async Server Components that `fetch()` data directly. Components needing interactivity/state (`app/page.jsx`, `CourseSearch`) are explicitly marked `'use client'`.
- **Two different data-fetching patterns are intentionally demonstrated side by side**:
  - `app/code/repos/**`: server-side fetching directly in Server Components, streamed into the page via nested `<Suspense>` boundaries (see `app/code/repos/[name]/page.jsx`).
  - `app/page.jsx` (home): client-side fetching in a `useEffect`, calling this app's own API routes.
- **API routes** (Route Handlers) live under `app/api/**/route.js` and export `GET`/`POST` functions returning `NextResponse` (or a raw `Response`). `app/api/courses` reads/mutates an in-memory array from `app/api/courses/data.json` — POST mutations are not persisted to disk and reset on server restart.
- **External data source**: the `/code/repos` section fetches live data from the GitHub REST API (`https://api.github.com/...bradtraversy...`) with Next's `fetch` cache/`revalidate: 60` option — no auth token, so it's subject to GitHub's unauthenticated rate limits. Artificial `setTimeout` delays exist in `Repo.jsx`/`RepoDirs.jsx` to demonstrate streaming/loading states.

## CI

`Jenkinsfile` runs an unrelated automated PR-review bot (`10pdocker/ai-pr-bot`) against pull requests — it does not build, lint, or test this app.
