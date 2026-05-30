---
name: upskilling-external-references
description: Use when the user asks to add, move, organize, summarize, or manage third-party learning material in the way-to-architect repo, including copied repositories, forked repos, course exports, source links, licenses, submodules, subtrees, screenshots, PDFs, or external reference indexes.
---

# Upskilling External References

Repo: `/Users/sujeithgopinath/Personal/way-to-architect/`

Primary folders:

- Topic-owned references: `<topic>/external/`
- Broad or uncategorized references: `external/`
- Coding interview imports: `Coding Interview/`
- React TypeScript copied repo: `frontend/react/external/react-typescript-cheatsheet/`

Workflow:

1. Keep copied third-party material under the nearest topic-local `external/`
   folder when it clearly belongs to one topic.
2. Add or update a topic-level `README.md` with source URL, local path, and
   license notes.
3. Do not mix full copied repos directly into curated folders such as
   `frontend/react/`; use `frontend/react/external/`.
4. Extract short notes in the user's own words into the matching curated folder.
5. Prefer `git submodule` or `git subtree` for repos that should keep upstream
   history or receive updates.
6. Keep large personal screenshot dumps under `external/screenshots/`; that
   folder is intentionally ignored by Git except for its README.
7. Keep broad PDFs, EPUBs, screenshots, and images in ignored external intake
   folders unless they have been reduced to curated notes.
8. Run `git diff --check` after edits.
