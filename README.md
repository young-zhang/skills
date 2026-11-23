# Young's Skills

A portable collection of agent skills and utilities for Claude Code, Codex
CLI, Google Antigravity, and GitHub Copilot CLI. The repository packages one
plugin, `yz`, for all four harnesses. Each skill follows the
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
| Copilot CLI | `yz`   | `young-zhang-skills` |

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

### GitHub Copilot CLI

Copilot CLI has no manifest of its own in this repository. It reads the Claude
Code files `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`.
Run these commands in the shell:

```sh
copilot plugin marketplace add young-zhang/skills
copilot plugin install yz@young-zhang-skills
```

Inside a session, `/plugin marketplace add young-zhang/skills` and
`/plugin install yz@young-zhang-skills` do the same. Start a new session. Then
invoke a skill with its plugin prefix:

```text
/yz:skill-name
```

Slash commands run only in an interactive session. `copilot -p` passes
`/yz:skill-name` to the model as plain text and does not load the skill. Type
`/skills list` in a session, or run `copilot skill list` in the shell, to see
the loaded skills. Run `copilot plugin list` to see the installed plugins.

`copilot plugin install young-zhang/skills` also installs the plugin, without
a marketplace. Copilot CLI marks this direct install as deprecated, so use the
marketplace commands above.

## Release changes

1. Change or add skills under `skills/`.
2. Set the next `yy.ddd.n` version in all three manifests in the same commit:
   `.claude-plugin/plugin.json`, `.codex-plugin/plugin.json`, and
   `.antigravity-plugin/plugin.json`. Claude Code keeps every user on the
   cached copy until this string changes, even after new commits. Copilot CLI
   reads the version in `.claude-plugin/plugin.json`.
3. Run the checks in [AGENTS.md](AGENTS.md#checks-before-a-commit).
4. Commit and push to the default branch on GitHub.

## Update

Run these steps on each machine that has the plugin installed, after you push
a new version.

### Update in Claude Code

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

### Update in Codex CLI

Run this command in the shell, then start a new session:

```sh
codex plugin marketplace upgrade young-zhang-skills
```

The command fetches the latest marketplace snapshot from GitHub and refreshes
the installed plugin from it. If a skill still shows its old text, copy the
plugin again:

```sh
codex plugin add yz@young-zhang-skills
```

`upgrade` works only for a marketplace added from GitHub. For a marketplace
added from a local path, run `codex plugin add yz@young-zhang-skills` instead.
Run `codex plugin list` to confirm the new version.

### Update in Antigravity

Antigravity has no update command for a plugin installed from a GitHub URL.
Run the install command again in the shell. Antigravity replaces the
installed copy. Then start a new conversation:

```sh
agy plugin install https://github.com/young-zhang/skills
```

Run `agy plugin list` to confirm the new version.

### Update in Copilot CLI

Run these commands in the shell, then start a new session:

```sh
copilot plugin marketplace update young-zhang-skills
copilot plugin update yz@young-zhang-skills
```

The first command refreshes the marketplace listing. The second command
installs the new version. `copilot plugin update --all` updates every
installed plugin. In a session, the `/plugin` dashboard marks a plugin that has
a newer version and offers an **Update** action. Run `copilot plugin list` to
confirm the new version.

Copilot CLI updates only its built-in `awesome-copilot` marketplace
automatically. To update this marketplace at the start of each session, set
`autoUpdate: true` on its entry in `extraKnownMarketplaces` in your Copilot
CLI user settings (`~/.copilot/settings.json`).

## Uninstall

### Uninstall from Codex CLI

Run these commands in the shell, in this order:

```sh
codex plugin remove yz@young-zhang-skills
codex plugin marketplace remove young-zhang-skills
```

The first command uninstalls the plugin and deletes its cached copy. The
second command removes the marketplace. Removing only the marketplace leaves
the `yz` plugin entry in `~/.codex/config.toml` and its copy in
`~/.codex/plugins/cache/`. Run `codex plugin marketplace list` to check what
is still configured.

### Uninstall from Antigravity

Run this command in the shell:

```sh
agy plugin uninstall yz
```

The command removes the plugin and its copy in `~/.gemini/config/plugins/yz/`.
Antigravity has no marketplace entry to remove for a plugin installed from a
GitHub URL. Run `agy plugin list` to confirm that `yz` is gone.

### Uninstall from Copilot CLI

Run these commands in the shell, in this order:

```sh
copilot plugin uninstall yz@young-zhang-skills
copilot plugin marketplace remove young-zhang-skills
```

The first command uninstalls the plugin and its skills. The second command
removes the marketplace. Copilot CLI refuses to remove a marketplace while a
plugin from it is still installed. `copilot plugin marketplace remove
young-zhang-skills --force` removes the marketplace and uninstalls its plugins
in one step. To turn the plugin off without removing it, run
`copilot plugin disable yz@young-zhang-skills`.

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
- Copilot CLI: `copilot --plugin-dir .` loads the working copy for that
  session only. `copilot --plugin-dir . plugin list` shows it under "External
  Plugins (via --plugin-dir)". To keep it installed instead, run
  `copilot plugin marketplace add .`, then
  `copilot plugin install yz@young-zhang-skills`. Copilot CLI does not copy a
  plugin from a local marketplace. It loads the skills from the working copy,
  so an edit takes effect in the next session.

Codex keeps one marketplace per name. If the GitHub marketplace is already
configured, run `codex plugin marketplace remove young-zhang-skills` before
you add the local path, and add the GitHub source again when you are done.
Copilot CLI also registers a marketplace by its name, so do the same with
`copilot plugin marketplace remove young-zhang-skills --force`.

To use one skill outside the plugin, without the `yz` prefix, link its
directory into a harness skills directory. The examples use the Claude Code
personal skills directory. Codex CLI reads `~/.agents/skills/`. The
Antigravity CLI reads `~/.gemini/config/skills/`. Copilot CLI reads
`~/.copilot/skills/` and `~/.agents/skills/`.

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
- Copilot CLI plugin reference:
  <https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-plugin-reference>
- Copilot CLI plugin creation:
  <https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-creating>
- Copilot CLI plugin marketplaces:
  <https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-marketplace>
- Copilot CLI skills:
  <https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-skills>

## License

The [MIT License](LICENSE) covers the original work in this repository. A
third-party skill is governed by the license in its own directory under
`skills/`, not by the MIT License.
