---
name: upskilling-system-design
description: Use when the user asks to add, revise, retrieve, or practice system design learning notes in the way-to-architect repo, including scalability, availability, rate limiting, queues, caches, estimation, distributed systems, chapter notes, case studies, or system design interview practice.
---

# Upskilling System Design

Repo: `/Users/sujeithgopinath/Personal/way-to-architect/`

Primary folders:

- System design notes and case studies: `system-design/`
- Platform and infrastructure product notes:
  `system-design/platform-engineering/`
- Numbered chapter imports: `system-design/01. Scaling/` through
  `system-design/28. Stock Exchange/`
- Databases and storage concepts: `databases/`
- Architecture background: `architectures/`
- Interview practice: `interviews/`, `Coding Interview/`

Workflow:

1. Search `system-design/`, `databases/`, and `architectures/` with `rg`.
2. Put reusable concepts under focused system-design topic folders.
3. Put imported chapter material under the numbered chapter folders, and curated
   case-study walkthroughs under matching topic or case-study folders.
4. Include requirements, scale assumptions, APIs/data model, high-level design,
   bottlenecks, tradeoffs, and failure modes.
5. For interview-style notes, keep diagrams and back-of-envelope estimates close
   to the written explanation.
6. For platform engineering notes, capture the developer-facing API, control
   plane, data plane, provisioning flow, migration path, operational ownership,
   and blast-radius/failure-mode analysis.
7. Run `git diff --check` after edits.
