---
name: upskilling-handbook
description: Use when the user asks to add, revise, organize, or retrieve personal learning notes in the way-to-architect repo, especially for software architecture, system design, databases, design patterns, JavaScript, TypeScript, React, Angular, frontend engineering, testing, coding practices, interviews, exercises, roadmaps, or external learning references.
---

# Upskilling Handbook

The user's curated learning repo is:

`/Users/sujeithgopinath/Personal/way-to-architect/`

Use this repo as the source of truth for personal upskilling notes. Keep detailed
knowledge in the repo, not in this skill.

## Workflow

1. Search the repo first with `rg` and read the closest `README.md` files.
2. Put new notes in the most specific existing folder.
3. If no folder fits, create a small `README.md` landing page and link it from
   the nearest parent index and the root `README.md` when useful.
4. Keep notes in the user's own words. Prefer concise explanations, examples,
   common mistakes, and decision rules.
5. For modern framework/API behavior, verify with official docs before writing
   durable guidance.
6. Use topic-local `external/` folders for material owned by one topic, such as
   `frontend/react/external/`. Use top-level `external/` for broad source dumps,
   duplicate imports, screenshots, or uncategorized references.
7. Do not copy large course exports, PDFs, or third-party repos into curated
   sections unless license and usefulness are clear.
8. Run `git diff --check` after edits.

## Navigation

- Root index and GitHub Pages config: `README.md`, `_config.yml`, `_includes/`
- Imported coding interview exports: `Coding Interview/`
- JavaScript: `languages/javascript/`
- TypeScript: `languages/typescript/`
- Architecture: `architectures/`
- System design: `system-design/`
- Databases: `databases/`
- Design patterns: `design-patterns/`
- Anti-patterns: `anti-patterns/`
- Enterprise applications: `enterprise/`
- Programming paradigms: `paradigms/`
- Design principles: `principles/`
- React: `frontend/react/`
- Angular: `frontend/angular/`
- Browser platform: `frontend/browser/`
- Frontend performance: `frontend/performance/`
- Client-side storage: `frontend/client-side-storage/`
- Frontend architecture: `frontend/areas/`, `frontend/atomic-design/`,
  `frontend/design-system/`, `frontend/metrics/`, `frontend/core-team/`,
  `frontend/legacy-lifecycle/`, `frontend/accessibility-testing/`
- Redux: `redux/`
- Testing: `testing/` and `coding-practices/testing/`
- Coding practices: `coding-practices/`
- Software engineering culture: `swe/`
- Interviews: `interviews/`
- Roadmaps and reading queues: `roadmaps/`
- Exercises: `exercises/`
- Operating systems: `os/`
- Models of computation: `models-of-computation/`
- Data processing: `ds/`
- Glossary: `glossary/`
- External references: `external/`

React external references live under `frontend/react/external/`. Keep copied
repositories there, then extract short curated notes into `frontend/react/`.

When adding exercise material, keep it under `exercises/` and include goal,
constraints, expected behavior, and solution notes after completion.
