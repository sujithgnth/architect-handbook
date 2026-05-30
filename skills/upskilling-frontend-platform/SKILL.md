---
name: upskilling-frontend-platform
description: Use when the user asks to add, revise, retrieve, or practice browser platform and frontend performance notes in the way-to-architect repo, including DOM, events, storage, cookies, service workers, CORS, CSP, rendering, Core Web Vitals, caching, accessibility, design systems, or browser APIs.
---

# Upskilling Frontend Platform

Repo: `/Users/sujeithgopinath/Personal/way-to-architect/`

Primary folders:

- Browser platform: `frontend/browser/`
- Client-side storage: `frontend/client-side-storage/`
- Frontend performance: `frontend/performance/`
- Accessibility: `frontend/accessibility-testing/`
- Design systems and frontend architecture: `frontend/design-system/`,
  `frontend/atomic-design/`, `frontend/areas/`, `frontend/core-team/`,
  `frontend/metrics/`, `frontend/legacy-lifecycle/`
- Redux notes: `redux/`
- Exercises: `exercises/`

Workflow:

1. Search existing notes with `rg`.
2. Put browser API and platform behavior under `frontend/browser/`.
3. Put storage-specific guidance under `frontend/client-side-storage/`.
4. Put performance measurement and optimization under `frontend/performance/`.
5. Verify modern browser behavior with MDN or official specs before writing
   durable guidance.
6. Run `git diff --check` after edits.
