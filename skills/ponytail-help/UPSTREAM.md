# Upstream

This skill is copied from the `ponytail-help` skill of the Ponytail plugin by
DietrichGebert.

- Repository: <https://github.com/DietrichGebert/ponytail>
- Source path: `skills/ponytail-help/SKILL.md`
- Commit: `ebd8654c2c627f721eb8f9c0d1bd4090a29f9e10` (2026-11-22)
- License: MIT. The upstream license text is in `LICENSE` in this directory.
- Checkout key: `UPSTREAM_DIETRICHGEBERT_PONYTAIL`. Set it in `.env` to the
  path of a local checkout of the repository. See `.env.example`.

## Changes

- `SKILL.md`: added the frontmatter line `license: MIT (see LICENSE)`.
- `SKILL.md`: replaced the paragraph on invocation names in Codex CLI,
  Claude Code, and OpenCode (`$ponytail:ponytail` and others) with the `yz`
  names for each harness (`/yz:ponytail`, `$yz:ponytail`, `/ponytail`), and
  added a paragraph that says the `yz` plugin has no hooks, so ponytail does
  not turn on by itself at session start.
- `SKILL.md`: removed the sections "Band (Claude Code)", "Configure Default
  Mode", and "Update". They describe features of the upstream hooks
  (the status band, `PONYTAIL_DEFAULT_MODE`, `~/.config/ponytail/config.json`,
  auto-activation) and the update steps of the upstream `ponytail` plugin.
  None of them work in the `yz` plugin.
- Not copied: the upstream plugin manifests and marketplace files, the
  Gemini CLI command `commands/ponytail-help.toml` (a shorter prompt with the
  same rules as `SKILL.md`), the copies of the skill in `.openclaw/skills/` and
  `.opencode/command/`, `skills/plugin.json`, the Node.js hooks in `hooks/`,
  the benchmarks, and the tests. The `yz` plugin manifests in this repository
  package the skill instead.
