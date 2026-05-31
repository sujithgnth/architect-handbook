---
name: upskilling-roadmaps
description: Use when the user asks to create, revise, organize, or follow learning roadmaps in the way-to-architect repo, including study plans, topic sequences, progress tracking, weekly plans, prerequisite maps, pending reads, interview prep plans, or converting notes into a learning path.
---

# Upskilling Roadmaps

Repo: `/Users/sujeithgopinath/Personal/way-to-architect/`

Preferred folder: `roadmaps/`

Related source folders: `README.md`, `interviews/`, `Coding Interview/`,
`exercises/`, `system-design/`, `frontend/`, `languages/`, `architectures/`,
`databases/`, `coding-practices/`, and topic-local `resources/` folders.

Workflow:

1. Search existing notes and folders with `rg` before creating a roadmap.
2. Use `roadmaps/README.md` as the index and `roadmaps/pending-reads.md` for
   unsorted reading queues.
3. Roadmaps should list objective, prerequisites, topic order, practice tasks,
   checkpoint questions, and source folders.
4. Keep plans short enough to execute. Prefer weekly milestones over large vague
   lists.
5. Link roadmap items to existing handbook notes and exercises.
6. Run `git diff --check` after edits.
