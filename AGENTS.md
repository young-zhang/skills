# Contributor Instructions

This repository is Young's Skills. It packages one plugin, `yz`, for Claude
Code, Codex CLI, Google Antigravity, and GitHub Copilot CLI. All four
harnesses load the same `skills/` directory at the repository root. These
instructions apply to every agent and person that changes the repository.

## Repository layout

| Path                               | Purpose                                                       |
| ---------------------------------- | ------------------------------------------------------------- |
| `skills/<skill-name>/SKILL.md`     | One packaged skill.                                           |
| `.claude-plugin/plugin.json`       | Claude Code and Copilot CLI plugin manifest.                  |
| `.claude-plugin/marketplace.json`  | Claude Code and Copilot CLI marketplace `young-zhang-skills`. |
| `.codex-plugin/plugin.json`        | Codex CLI plugin manifest.                                    |
| `.agents/plugins/marketplace.json` | Codex CLI marketplace `young-zhang-skills`.                   |
| `.antigravity-plugin/plugin.json`  | Antigravity plugin manifest.                                  |
| `AGENTS.md`                        | These shared instructions.                                    |
| `CLAUDE.md`                        | Imports `AGENTS.md` for Claude Code.                          |
| `.agents/skills/<name>/SKILL.md`   | Repository-only skill. Not packaged.                          |
| `.claude/skills/<name>/SKILL.md`   | Claude Code pointer to a repository-only skill.               |
| `.env.example`                     | Template for `.env`: local upstream checkouts.                |

The repository root is the plugin root for all four harnesses. Each manifest
refers to `skills/` with a relative path or with the harness default:

- Claude Code, Codex CLI, and Copilot CLI: `"skills": "./skills/"`.
- Antigravity: no `skills` field. Antigravity always loads `skills/` at the
  plugin root, and its `skills` field only adds extra directories.

Copilot CLI has no manifest of its own here. It looks for a plugin manifest at
`.plugin/plugin.json`, `plugin.json`, `.github/plugin/plugin.json`, and
`.claude-plugin/plugin.json`, in this order, and uses the first one it finds.
It looks for a marketplace at `marketplace.json`, `.plugin/marketplace.json`,
`.github/plugin/marketplace.json`, and `.claude-plugin/marketplace.json`, in
the same way. Do not add a file at any of the earlier paths. Copilot CLI would
read it in place of the `.claude-plugin/` file.

Do not add a second directory of packaged skills, a copy of a skill, or a
symbolic link to a skill. A new directory under `skills/` is packaged for
every harness with no manifest change.

## Repository-only skills

Some skills help maintain this repository and are not for plugin users. They
are not packaged in the `yz` plugin:

| Skill            | What it does                                                                |
| ---------------- | --------------------------------------------------------------------------- |
| `check-upstream` | Compares copied skills with their upstream source and offers updates.       |
| `import-skill`   | Imports a third-party skill with the steps in "Adapted third-party skills". |

Each one lives at `.agents/skills/<name>/SKILL.md`. Codex CLI, Antigravity,
and Copilot CLI load skills from `.agents/skills/` in the workspace. Claude
Code does not, so each skill also has a short pointer at
`.claude/skills/<name>/SKILL.md`. The pointer has the same frontmatter and one
instruction: read and follow the file in `.agents/skills/`. Keep the full
instructions only in `.agents/skills/`.

Copilot CLI also reads `.claude/skills/`. It reads `.agents/skills/` first and
keeps the first skill it finds for each name, so it uses the full
instructions and skips the pointer. Keep the `name` field of the pointer equal
to the `name` field of the full skill, or Copilot CLI loads both.

Invoke them as `/<name>` in Claude Code, Antigravity, and Copilot CLI, and as
`$<name>` in Codex CLI. In Copilot CLI, slash commands run only in an
interactive session, not with `copilot -p`. A skill that must run only when
invoked sets `disable-model-invocation: true` in its frontmatter (Claude Code,
Antigravity, and Copilot CLI) and has `agents/openai.yaml` with
`policy: allow_implicit_invocation: false` (Codex CLI).

## Skills

### Where a skill lives

Put each skill in its own directory: `skills/<skill-name>/SKILL.md`. Do not
nest skills inside other skills. Do not put a `SKILL.md` at the repository
root.

### Portable skill names

Follow the Agent Skills specification
(<https://agentskills.io/specification>), so that one name works in every
harness:

- Use 1 to 64 characters: lowercase letters `a-z`, digits `0-9`, and hyphens.
- Do not start or end the name with a hyphen. Do not use two hyphens in a row.
- Make the `name` field in the frontmatter equal to the directory name.
- Pick a distinct name. Claude Code and Copilot CLI show the skill as
  `/yz:<skill-name>` and Codex CLI shows it as `yz:<skill-name>`, but
  Antigravity shows it as
  `/<skill-name>` with no plugin prefix. A generic name such as `review`,
  `help`, `init`, `plugin`, or `skills` can collide with a built-in command or
  another plugin in Antigravity.

### Frontmatter

Every `SKILL.md` starts with YAML frontmatter that has `name` and
`description`:

```markdown
---
name: example-skill
description: Converts X to Y. Use when the user asks to convert X files or mentions Y.
---

Instructions for the agent go here.
```

- `description` must be 1 to 1024 characters. Say what the skill does and when
  to use it. The harnesses decide when to load a skill from this text.
- Use only fields from the Agent Skills specification (`name`, `description`,
  `license`, `compatibility`, `metadata`, `allowed-tools`) unless a field is
  needed for one harness. A harness-specific field must not break the others.
- Keep `SKILL.md` under 500 lines. Move long material into supporting files.

### Supporting files

Keep every file that a skill needs inside the skill directory:

```text
skills/<skill-name>/
  SKILL.md
  scripts/      code the agent runs
  references/   documents the agent reads on demand
  assets/       templates, images, and other static files
```

- Refer to supporting files from `SKILL.md` with relative paths, for example
  `scripts/convert.py` or `references/format.md`. Keep references one level
  deep from `SKILL.md`.
- Never use absolute paths or paths outside the skill directory. Each harness
  installs a copy of the plugin in its own cache directory.
- Scripts must run on Windows, macOS, and Linux. Prefer Python or Node.js. If
  you add a shell script, also give a PowerShell or Python alternative, or
  state in `SKILL.md` that the script needs a POSIX shell.
- Give shell scripts LF line endings (`.gitattributes` enforces this) and a
  shebang line. Mark them executable in Git with
  `git update-index --chmod=+x <path>`, because Windows checkouts do not keep
  the file mode.
- Do not commit generated files, caches, secrets, or credentials.

## Adapted third-party skills

The MIT license in `LICENSE` covers only original work in this repository. A
skill copied or adapted from another project stays under its upstream license.
`/import-skill <description>` does the steps below and asks you to confirm the
plan before it writes any file. Describe the skill in plain text and give the
path of your local checkout if you have one, for example
`/import-skill the reprompt skill from G:/github/pronskiy-skills`. To add one
by hand:

1. Confirm that the upstream license allows redistribution and modification.
   Do not add a skill that has no license or a license that forbids this.
2. Copy the upstream license text, unchanged, into the skill directory as
   `LICENSE` (or the file name the upstream project uses).
3. If the upstream project has a `NOTICE` file or other required notices,
   copy them unchanged into the skill directory. The Apache License 2.0, for
   example, requires that you keep the `NOTICE` text.
4. Keep every copyright, author, and attribution line from the upstream
   files. Do not remove or change them.
5. Add `UPSTREAM.md` to the skill directory. `/check-upstream` reads it, so
   use these labels as list items: `Repository:` (the Git URL),
   `Source path:` (the upstream path of the skill's `SKILL.md`), `Commit:`
   (the full commit SHA you copied from), `License:` (the SPDX license
   identifier), and `Checkout key:` (the `.env` key for the repository, in
   the form `UPSTREAM_<OWNER>_<REPO>` that `.env.example` explains). Under a
   `## Changes` heading, list every change you made. See
   `skills/reprompt/UPSTREAM.md` for an example.
6. If `.env.example` has no line for the checkout key, add one: a comment
   with the repository URL and the skills that use it, then the key with no
   value. Each person copies `.env.example` to `.env` (not committed) and sets
   the paths of their local checkouts, so `/check-upstream` can use them
   instead of a temporary clone.
7. Follow any rule in the upstream license about changed files. The Apache
   License 2.0, section 4(b), requires a prominent notice in each modified
   file that states that you changed it. A list in `UPSTREAM.md` alone does
   not meet that rule.
8. Set the `license` frontmatter field in `SKILL.md`, for example
   `license: Apache-2.0 (see LICENSE)`.

## Versions and releases

The three plugin manifests carry the same `version`. Change all three in the
same commit:

- `.claude-plugin/plugin.json` (also read by Copilot CLI)
- `.codex-plugin/plugin.json`
- `.antigravity-plugin/plugin.json`

Every commit sets a new version of the form `yy.ddd.n`:

- `yy` is the last two digits of the year, for example `26` for 2026.
- `ddd` is the day of the year, counted from `0` for January 1. November 22,
  2026 is `325`.
- `n` counts the commits made on that day, starting from `0`. The first
  commit of the day is `0`, the next is `1`, and so on.

Use the local date of the commit. If the current version already starts with
today's `yy.ddd`, increase `n` by one. If not, set `n` to `0`. Do not add
leading zeros: write `26.4.0` for January 5, 2026, not `26.004.0`. Leading
zeros are not valid in Semantic Versioning, which Antigravity recommends for
`version`.

To print today's `yy.ddd`:

```sh
echo "$(date +%y).$(( 10#$(date +%j) - 1 ))"
```

```powershell
"{0:yy}.{1}" -f (Get-Date), ((Get-Date).DayOfYear - 1)
```

Claude Code does not offer an update to users until this string changes. Do
not set `version` in `.claude-plugin/marketplace.json`. A version in the
marketplace entry conflicts with the manifest version.

## Checks before a commit

Run these from the repository root:

```sh
claude plugin validate .
node -e "for (const f of ['.claude-plugin/plugin.json','.claude-plugin/marketplace.json','.codex-plugin/plugin.json','.agents/plugins/marketplace.json','.antigravity-plugin/plugin.json']) JSON.parse(require('fs').readFileSync(f,'utf8'))"
```

If Antigravity is installed, also run `agy plugin validate .`.

If Copilot CLI is installed, also run `copilot --plugin-dir . plugin list`.
Copilot CLI has no validate command. The output must list `yz` with the new
version under "External Plugins (via --plugin-dir)".

`claude plugin validate .` reports a warning that `CLAUDE.md` at the plugin
root is not loaded as plugin context. `agy plugin validate .` reports a
warning that `AGENTS.md` is never loaded, because Antigravity plugins load
rules only from `rules/`. Both warnings are expected. `AGENTS.md` and
`CLAUDE.md` are for people and agents that work in this repository, not for
plugin users.
