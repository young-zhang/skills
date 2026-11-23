# Upstream

This skill is copied from the `pua-ja` skill of the PUA plugin by
探微安全实验室 (TanWei Security Lab).

- Repository: <https://github.com/tanweai/pua>
- Source path: `skills/pua-ja/SKILL.md`
- Commit: `e6e6cd237ad17750d179674bff52f8184abea8fd` (2026-09-09)
- License: MIT. The license text is in `LICENSE` in this directory.
  The upstream repository has no license file; see "Changes".
- Checkout key: `UPSTREAM_TANWEAI_PUA`. Set it in `.env` to the
  path of a local checkout of the repository. See `.env.example`.

## Changes

- Added `LICENSE`: the standard MIT License text with the copyright line
  "Copyright (c) 2026 探微安全实验室 (TanWei Security Lab)". The upstream repository
  has no license file at this commit or in its history. It declares MIT in the
  "License" section of `README.md`, in the `license` field of
  `.claude-plugin/plugin.json`, and in the `license` field of each `SKILL.md`.
  The author name and the year come from the `author` field of
  `.claude-plugin/plugin.json` and the date of the first upstream commit
  (2026-03-09).
- The rest of the files are unchanged. The `SKILL.md` frontmatter keeps the
  upstream `license: MIT` line.
- Not copied: the upstream plugin manifests and marketplace files, the hooks in
  `hooks/`, the slash commands in `commands/`, the agents in `agents/`, the
  scripts in `scripts/`, the files for other harnesses (`codex/`, `kimi/`,
  `hermes/`, `.trae/`, `trae/`, `codebuddy/`, `chatgpt/`, `pi/`, `cursor/`,
  `kiro/`, `vscode/`), the `pua-loop` skill, which needs the upstream Stop hook
  and `scripts/setup-pua-loop.sh`, and the evals and tests. The `yz` plugin
  manifests in this repository package the skill instead.
