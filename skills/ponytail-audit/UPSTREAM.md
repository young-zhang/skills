# Upstream

This skill is copied from the `ponytail-audit` skill of the Ponytail plugin by
DietrichGebert.

- Repository: <https://github.com/DietrichGebert/ponytail>
- Source path: `skills/ponytail-audit/SKILL.md`
- Commit: `ebd8654c2c627f721eb8f9c0d1bd4090a29f9e10` (2026-11-22)
- License: MIT. The upstream license text is in `LICENSE` in this directory.
- Checkout key: `UPSTREAM_DIETRICHGEBERT_PONYTAIL`. Set it in `.env` to the
  path of a local checkout of the repository. See `.env.example`.

## Changes

- `SKILL.md`: added the frontmatter line `license: MIT (see LICENSE)`. The rest
  of the file is unchanged.
- Not copied: the upstream plugin manifests and marketplace files, the
  Gemini CLI command `commands/ponytail-audit.toml` (a shorter prompt with the
  same rules as `SKILL.md`), the copies of the skill in `.openclaw/skills/` and
  `.opencode/command/`, `skills/plugin.json`, the Node.js hooks in `hooks/`,
  the benchmarks, and the tests. The `yz` plugin manifests in this repository
  package the skill instead.
