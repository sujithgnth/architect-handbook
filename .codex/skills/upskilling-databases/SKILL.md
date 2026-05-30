---
name: upskilling-databases
description: Use when the user asks to add, revise, retrieve, or practice database learning notes in the way-to-architect repo, including relational databases, transactions, isolation, replication, partitioning, CAP, consistency, GraphQL, wide-column stores, quorum consensus, failure handling, data modeling, or storage tradeoffs.
---

# Upskilling Databases

Repo: `/Users/sujeithgopinath/Personal/way-to-architect/`

Primary folders:

- Database concepts: `databases/`
- Relational details: `databases/relational/`
- Distribution and consistency: `databases/cap/`, `databases/consistency/`,
  `databases/consistency-models/`, `databases/quorum-consensus/`
- Scaling: `databases/scaling/`, `databases/partitioning/`,
  `databases/replication/`, `databases/handling-failures/`
- Data access patterns: `design-patterns/data/`
- Object-relational patterns: `design-patterns/object-relational/`
- Storage-heavy system designs: `system-design/`

Workflow:

1. Search existing database notes with `rg`.
2. Put general concepts under `databases/`.
3. Put application data-access patterns under `design-patterns/data/` or
   `design-patterns/object-relational/`.
4. Include when to use it, when to avoid it, tradeoffs, failure modes, and short
   examples.
5. For product-specific or version-sensitive behavior, verify with official
   vendor documentation before writing durable guidance.
6. Run `git diff --check` after edits.
