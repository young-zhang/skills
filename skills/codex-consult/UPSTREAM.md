# Upstream

This skill is copied from the `consult` skill of the Fusion Harness plugin by
Roman Pronskiy (<https://pronskiy.com>).

- Repository: <https://github.com/pronskiy/skills>
- Source path: `plugins/fusion-harness/skills/consult/SKILL.md`
- Commit: `4c3ee60d486e83bd14d4b12699f3c4cbe14b15bf` (2026-10-15)
- License: MIT. The upstream license text is in `LICENSE` in this directory.
- Checkout key: `UPSTREAM_PRONSKIY_SKILLS`. Set it in `.env` to the path of a
  local checkout of the repository. See `.env.example`.

## Changes

- Renamed the skill from `consult` to `codex-consult`: the directory and the
  `name` frontmatter field. Antigravity shows skills with no plugin prefix,
  so a generic name such as `consult` can collide with a skill from another
  plugin.
- `SKILL.md`: added the frontmatter line `license: MIT (see LICENSE)`.
- `SKILL.md`: added `< /dev/null` to the end of the `codex exec` command in
  "Procedure", and added this sentence to "Prerequisite": "Run `codex exec`
  with stdin redirected from `/dev/null`. If stdin stays open, Codex waits to
  read from it and stops responding." When stdin is open, `codex exec` waits
  to read more input from it, so the command never finishes.
- The rest of `SKILL.md` is unchanged.
- Not copied: the upstream plugin manifest
  (`plugins/fusion-harness/.claude-plugin/plugin.json`) and the Watchdog
  feature (`commands/watchdog.md`, `hooks/hooks.json`, and `scripts/`). The
  Watchdog is a Claude Code `Stop` hook, not a skill, and the `yz` plugin
  packages only skills. The `yz` plugin manifests in this repository package
  the skill instead.
