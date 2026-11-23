---
name: check-upstream
description: Checks each skill in skills/ that was copied from another project against its upstream source, reports the upstream changes and whether they are worth taking, and offers to update the local copy. Use when asked to check upstream, sync third-party skills, or find out whether a copied skill changed upstream. Works only in the Young's Skills repository.
disable-model-invocation: true
---

Read `.agents/skills/check-upstream/SKILL.md` at the repository root and follow
its instructions. That file is the only copy of this skill. Codex CLI,
Antigravity, and Copilot CLI load it from `.agents/skills/`, and Claude Code
loads this file.
