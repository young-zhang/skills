# Upstream

This skill is copied from the `reprompt` plugin by Roman Pronskiy
(<https://pronskiy.com>).

- Repository: <https://github.com/pronskiy/skills>
- Source path: `plugins/reprompt/skills/reprompt/SKILL.md`
- Commit: `4c3ee60d486e83bd14d4b12699f3c4cbe14b15bf` (2026-10-15)
- License: MIT. The upstream license text is in `LICENSE` in this directory.

## Changes

- `SKILL.md`: added the frontmatter line `license: MIT (see LICENSE)`. The rest
  of the file is unchanged.
- Added `agents/openai.yaml` with `allow_implicit_invocation: false`. Codex CLI
  ignores the `disable-model-invocation` frontmatter field, so without this
  file Codex could start the skill on its own. With it, the skill runs only
  when invoked, as upstream intends.
- Not copied: the upstream plugin manifest
  (`plugins/reprompt/.claude-plugin/plugin.json`) and the eval cases
  (`plugins/reprompt/tests/evals.json`). The `yz` plugin manifests in this
  repository package the skill instead.
