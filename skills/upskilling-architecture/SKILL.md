---
name: upskilling-architecture
description: Use when the user asks to add, revise, retrieve, or organize software architecture learning notes in the way-to-architect repo, including clean architecture, hexagonal architecture, DDD, CQRS, REST, microservices, SOA, layering, boundaries, enterprise architecture, tradeoffs, or architecture decision material.
---

# Upskilling Architecture

Repo: `/Users/sujeithgopinath/Personal/way-to-architect/`

Primary folders:

- Architecture styles and principles: `architectures/`
- Design patterns: `design-patterns/`
- Architecture principles: `principles/`
- Programming paradigms: `paradigms/`
- Enterprise applications: `enterprise/`
- Anti-patterns: `anti-patterns/`
- Glossary: `glossary/`
- Related engineering culture: `swe/`

Workflow:

1. Search existing notes with `rg`.
2. Put architecture-style notes under the matching `architectures/<topic>/`.
3. Put reusable implementation patterns under `design-patterns/`.
4. Put enterprise application material under `enterprise/`, and language-agnostic
   programming-model material under `paradigms/`.
5. Capture tradeoffs, context, alternatives, and consequences, not just
   definitions.
6. For significant decisions, suggest an ADR-style note if the user is deciding
   something durable.
7. Run `git diff --check` after edits.
