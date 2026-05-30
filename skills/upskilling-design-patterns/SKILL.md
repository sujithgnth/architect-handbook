---
name: upskilling-design-patterns
description: Use when the user asks to add, revise, retrieve, compare, or practice design pattern notes in the way-to-architect repo, including GoF patterns, enterprise patterns, base patterns, data patterns, object-relational patterns, distribution patterns, web presentation, session state, offline concurrency, anti-patterns, or pattern selection tradeoffs.
---

# Upskilling Design Patterns

Repo: `/Users/sujeithgopinath/Personal/way-to-architect/`

Primary folders:

- Patterns: `design-patterns/`
- Anti-patterns: `anti-patterns/`
- Principles: `principles/`
- OOP background: `paradigms/oop/`
- Architecture links: `architectures/`

Workflow:

1. Search existing pattern notes with `rg`.
2. Put notes under the narrowest matching pattern family: `base/`,
   `architectural/`, `data/`, `domain-logic/`, `object-relational/`,
   `distribution/`, `web-presentation/`, `session-state/`, or
   `offline-concurrency/`.
3. A good pattern note should include intent, problem, solution shape,
   consequences, when to use, when to avoid, and a small example.
4. Compare patterns when their boundaries are easy to confuse.
5. Link to related principles and architecture notes when useful.
6. Run `git diff --check` after edits.
