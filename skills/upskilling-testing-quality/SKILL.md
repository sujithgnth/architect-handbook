---
name: upskilling-testing-quality
description: Use when the user asks to add, revise, retrieve, or practice testing and quality notes in the way-to-architect repo, including unit tests, integration tests, E2E tests, test strategy, frontend testing, flaky tests, quality gates, code review quality, or regression testing.
---

# Upskilling Testing Quality

Repo: `/Users/sujeithgopinath/Personal/way-to-architect/`

Primary folders:

- Testing references: `testing/`
- Practical testing habits: `coding-practices/testing/`
- Code review quality checks: `coding-practices/code-review/`
- Frontend testing context: `frontend/accessibility-testing/`,
  `frontend/react/`, `frontend/angular/`
- Exercises: `exercises/`

Workflow:

1. Search existing testing and quality notes with `rg`.
2. Put broad testing concepts under `testing/`.
3. Put day-to-day testing habits under `coding-practices/testing/`.
4. Include what risk the test catches, what it should not cover, and common
   sources of brittleness.
5. For framework-specific testing, cross-link from React, Angular, or frontend
   notes.
6. Run `git diff --check` after edits.
