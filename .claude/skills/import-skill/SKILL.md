---
name: import-skill
description: Imports a skill from another project into skills/ with its license, required notices, UPSTREAM.md, checkout key, and .env.example line, following the adapted-skill rules in AGENTS.md. Use when asked to import, copy, adopt, or port a third-party skill into this repository. Works only in the Young's Skills repository.
argument-hint: "[skill description, with the local checkout path if you have one]"
disable-model-invocation: true
---

Read `.agents/skills/import-skill/SKILL.md` at the repository root and follow
its instructions, with the arguments given to this skill as the request. That
file is the only copy of this skill. Codex CLI and Antigravity load it from
`.agents/skills/`, and Claude Code loads this file.
