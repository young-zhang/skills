# Upstream

This skill is copied from the `duel-review` skill of the Fusion Harness plugin
by Roman Pronskiy (<https://pronskiy.com>).

- Repository: <https://github.com/pronskiy/skills>
- Source path: `plugins/fusion-harness/skills/duel-review/SKILL.md`
- Commit: `4c3ee60d486e83bd14d4b12699f3c4cbe14b15bf` (2026-10-15)
- License: MIT. The upstream license text is in `LICENSE` in this directory.
- Checkout key: `UPSTREAM_PRONSKIY_SKILLS`. Set it in `.env` to the path of a
  local checkout of the repository. See `.env.example`.

## Changes

- `SKILL.md`: added the frontmatter line `license: MIT (see LICENSE)`.
- `SKILL.md`: added `< /dev/null` to the end of both `codex exec` commands in
  "Procedure", and added this sentence to "Prerequisites": "Run `codex exec`
  with stdin redirected from `/dev/null`. If stdin stays open, Codex waits to
  read from it and stops responding." When stdin is open, `codex exec` waits
  to read more input from it, so the command never finishes. In both
  commands, the redirect comes after the closing `)"`. The heredoc inside
  `$(...)` gives input only to `cat`, not to `codex`.
- `SKILL.md`: added support for GitLab merge requests through the `glab`
  CLI. Upstream supports only GitHub pull requests through `gh`.
  - `description`: names GitLab merge requests, adds the trigger
    "duel review this MR", and says that the skill needs `gh` for GitHub or
    `glab` for GitLab, in place of "Requires the codex and gh CLIs".
  - "Prerequisites": `codex` is always required. `gh` is required only for
    a GitHub pull request, and `glab` only for a GitLab merge request
    (checked with `glab auth status`).
  - "Procedure", step 1: picks `gh` or `glab` from the link, or else from
    the host of the `origin` remote, and asks the user if the host is not
    clear. For GitLab it runs
    `glab mr view <mr> --output json --jq '{title, description, author: .author.username, target_branch, source_branch}'`
    and `glab mr diff <mr> --raw`. `glab mr view` and `glab mr diff` take a
    merge request number or a branch, not a link, so for a link the step
    passes the number and `-R <project URL>`. These commands were checked
    with glab 1.130.0.
- The rest of `SKILL.md` is unchanged. Its `description` field and "When to
  use" still name the Watchdog, which this repository does not include.
- Not copied: the upstream plugin manifest
  (`plugins/fusion-harness/.claude-plugin/plugin.json`) and the Watchdog
  feature (`commands/watchdog.md`, `hooks/hooks.json`, and `scripts/`). The
  Watchdog is a Claude Code `Stop` hook, not a skill, and the `yz` plugin
  packages only skills. The `yz` plugin manifests in this repository package
  the skill instead.
