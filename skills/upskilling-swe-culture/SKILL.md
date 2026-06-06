---
name: upskilling-swe-culture
description: Use when the user asks to add, revise, retrieve, or organize software engineering culture notes in the way-to-architect repo, including engineering practices, Google Software Engineering notes, XP, Unix philosophy, laws, team practices, maintainability, technical debt, architecture culture, or process lessons.
---

# Upskilling Software Engineering Culture

Repo: `/Users/sujeithgopinath/Personal/way-to-architect/`

Primary folders:

- Software engineering culture: `swe/`
- Platform ownership and maintenance: `swe/platform-engineering/`
- Architecture concerns: `architectures/general/`
- Coding practices: `coding-practices/`
- Principles: `principles/`
- Enterprise team and application context: `enterprise/`

Workflow:

1. Search existing notes with `rg`.
2. Put culture/process notes under `swe/`.
3. Put concrete coding habits under `coding-practices/`.
4. Preserve practical takeaways: what to do, why it matters, and where it fails.
5. Link laws, principles, and examples together when they explain the same
   tradeoff.
6. For platform ownership notes, include onboarding, on-call, migration,
   operational knowledge, churn signals, and team-facing communication.
7. Run `git diff --check` after edits.
