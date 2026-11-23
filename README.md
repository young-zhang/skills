# Young's Skills

A portable collection of agent skills and utilities for Claude Code, Codex
CLI, and Google Antigravity. The repository packages one plugin, `yz`, for all
three harnesses. Each skill follows the
[Agent Skills specification](https://agentskills.io/specification) and lives
at `skills/<skill-name>/SKILL.md`.

The GitHub install commands below work after the repository is published at
<https://github.com/young-zhang/skills>. The invocation examples are patterns:
replace `skill-name` with the name of a skill from the list below.

## Skills

| Skill      | What it does                                                          | Source                                                                | License |
| ---------- | --------------------------------------------------------------------- | --------------------------------------------------------------------- | ------- |
| `reprompt` | Rewrites a prompt to be more ambitious. Runs only when you invoke it. | Roman Pronskiy, [pronskiy/skills](https://github.com/pronskiy/skills) | MIT     |

A skill copied from another project records its source and changes in
`UPSTREAM.md` in its own directory.

## Install

| Harness     | Plugin | Marketplace          |
| ----------- | ------ | -------------------- |
| Claude Code | `yz`   | `young-zhang-skills` |
| Codex CLI   | `yz`   | `young-zhang-skills` |
| Antigravity | `yz`   | none (GitHub URL)    |

Versions use the form `yy.ddd.n`. See
[AGENTS.md](AGENTS.md#versions-and-releases) for the rule.

### Claude Code

Type these commands in a Claude Code session, not in the shell:

```text
/plugin marketplace add young-zhang/skills
/plugin install yz@young-zhang-skills
```

Run `/reload-plugins` or start a new session. Then invoke a skill with its
plugin prefix:

```text
/yz:skill-name
```

Claude Code also loads a skill on its own when a request matches the skill
description.

### Codex CLI

Run these commands in the shell:

```sh
codex plugin marketplace add young-zhang/skills
codex plugin add yz@young-zhang-skills
```

Start a new Codex session. To find a bundled skill, type `/skills` to list the
available skills, or type `$` to open the skill picker. Plugin skills show
with the plugin prefix, as `yz:skill-name`.

### Google Antigravity

Run this command in the shell:

```sh
agy plugin install https://github.com/young-zhang/skills
```

Start a new conversation. Then invoke a skill by its name, with no plugin
prefix:

```text
/skill-name
```

Antigravity also loads a skill on its own when a request matches the skill
description. Run `agy plugin list` to see the installed plugins.

## Release changes

1. Change or add skills under `skills/`.
2. Set the next `yy.ddd.n` version in all three manifests in the same commit:
   `.claude-plugin/plugin.json`, `.codex-plugin/plugin.json`, and
   `.antigravity-plugin/plugin.json`. Claude Code keeps every user on the
   cached copy until this string changes, even after new commits.
3. Run the checks in [AGENTS.md](AGENTS.md#checks-before-a-commit).
4. Commit and push to the default branch on GitHub.

## Refresh an install on another machine

Claude Code does not update third-party marketplaces automatically by
default. Run these commands in the shell, then run `/reload-plugins` in an
open session or start a new one:

```sh
claude plugin marketplace update young-zhang-skills
claude plugin update yz@young-zhang-skills
```

The first command refreshes the marketplace listing. The second command
installs the new version. To enable automatic updates for this marketplace,
open `/plugin` in a session, go to the Marketplaces tab, and enable
auto-update.

Codex CLI: run this command in the shell, then start a new session:

```sh
codex plugin marketplace upgrade young-zhang-skills
```

The command fetches the latest marketplace snapshot from GitHub and refreshes
the installed plugin from it. If a skill still shows its old text, run
`codex plugin add yz@young-zhang-skills` again to copy the plugin again.

Antigravity has no update command for a plugin installed from a GitHub URL.
Run the install command again in the shell. Antigravity replaces the
installed copy. Then start a new conversation:

```sh
agy plugin install https://github.com/young-zhang/skills
```

Run `agy plugin list` to confirm the new version.

## Local development

The plugin needs no links. Every manifest uses the real `skills/` directory
through a relative path, so a plain clone works on Windows, macOS, and Linux.
To test a working copy before you push, run these commands in the shell from
the repository root:

- Claude Code: `claude --plugin-dir .` loads the working copy for that session
  only. After you edit a skill, run `/reload-plugins` in the session.
- Codex CLI: `codex plugin marketplace add .`, then
  `codex plugin add yz@young-zhang-skills`. Codex copies the plugin into its
  cache. After you edit a skill, run `codex plugin add yz@young-zhang-skills`
  again.
- Antigravity: `agy plugin install .` copies the plugin into the Antigravity
  profile. After you edit a skill, run the command again. Antigravity replaces
  the installed copy.

Codex keeps one marketplace per name. If the GitHub marketplace is already
configured, run `codex plugin marketplace remove young-zhang-skills` before
you add the local path, and add the GitHub source again when you are done.

To use one skill outside the plugin, without the `yz` prefix, link its
directory into a harness skills directory. The examples use the Claude Code
personal skills directory. Codex CLI reads `~/.agents/skills/`. The
Antigravity CLI reads `~/.gemini/config/skills/`.

macOS and Linux:

```sh
mkdir -p ~/.claude/skills
ln -s "$PWD/skills/skill-name" ~/.claude/skills/skill-name
```

Windows, in PowerShell. A directory junction needs no administrator rights
and no Developer Mode:

```powershell
New-Item -ItemType Directory -Force -Path "$HOME\.claude\skills" | Out-Null
New-Item -ItemType Junction -Path "$HOME\.claude\skills\skill-name" -Target "$PWD\skills\skill-name"
```

If links are not available, copy the directory instead, and copy it again
after each change. Remove the link or copy when you are done, so that the
skill does not load twice.

## Contributing

Read [AGENTS.md](AGENTS.md) before you add a skill. It covers where skills
live, portable skill names, supporting files, and attribution for adapted
third-party skills.

## Documentation

- Agent Skills specification: <https://agentskills.io/specification>
- Claude Code plugins: <https://code.claude.com/docs/en/plugins>
- Claude Code plugin install and updates:
  <https://code.claude.com/docs/en/discover-plugins>
- Claude Code plugin marketplaces:
  <https://code.claude.com/docs/en/plugin-marketplaces>
- Claude Code skills: <https://code.claude.com/docs/en/skills>
- Codex CLI plugins: <https://developers.openai.com/codex/plugins/build>
- Codex CLI skills: <https://developers.openai.com/codex/skills>
- Antigravity plugins: <https://antigravity.google/docs/plugins/>
- Antigravity plugin manifest: <https://antigravity.google/docs/build-plugins/>
- Antigravity skills: <https://antigravity.google/docs/skills/>

## License

The [MIT License](LICENSE) covers the original work in this repository. A
third-party skill is governed by the license in its own directory under
`skills/`, not by the MIT License.
