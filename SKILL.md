---
name: way-to-architect-skill-pack
description: Use when installing, syncing, or selecting reusable skills from the way-to-architect repository for Codex work across the 2026-2028 career mission, architecture, system design, frontend, languages, databases, testing, roadmaps, exercises, and learning references.
---

# Way To Architect Skill Pack

This repository carries reusable Codex skill definitions for working with the
way-to-architect handbook and the 2026-2028 career mission.

Visible skill sources live in `skills/`. The local Codex runtime mirror lives in
`.codex/skills/`.

To use these skills in another Codex environment, copy the desired skill folders
into that environment's skill directory:

```sh
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
rsync -a skills/ "${CODEX_HOME:-$HOME/.codex}/skills/"
```

After copying, start a new Codex thread or refresh skill discovery so the updated
skill metadata is loaded.
