---
name: update-global
description: Installs the yz preferences file (assets/YZ_PREF.md) into the global instructions of Claude Code, Codex CLI, Antigravity, or Copilot CLI. Copies the file to a fixed path in the harness home folder and adds a marked block to the global instructions file that imports the copy, or holds the full text where the harness cannot import a file. Runs again safely to refresh the preferences. Runs only when invoked explicitly, with an optional harness name.
argument-hint: "[claude|codex|antigravity|copilot|all]"
disable-model-invocation: true
compatibility: Claude Code, Codex CLI, Antigravity, and Copilot CLI. Writes files in the home folder of each selected harness.
---

# Update Global

Install `assets/YZ_PREF.md` from this skill directory into the global
instructions of one or more harnesses. The global instructions file is the
file that a harness loads in every session, in every project.

## Why the skill copies the file

Do not make a global instructions file refer to `assets/YZ_PREF.md` in the
plugin install folder. That path is not stable:

- Claude Code and Codex CLI install the plugin in a folder named after the
  plugin version, for example `plugins/cache/young-zhang-skills/yz/26.326.3/`.
  Each update makes a new folder, so a path to the old one goes stale.
- Codex CLI cannot import a file. Copilot CLI does not load an import with an
  absolute path or a path that starts with `~/`.

So copy the file to a fixed path in the harness home folder, and refer to
that copy. Run this skill again after `YZ_PREF.md` changes.

## 1. Select the harnesses

- An argument of `claude`, `codex`, `antigravity`, or `copilot` selects that
  harness. `all` selects all four.
- With no argument, select the harness that runs this skill. If you cannot
  tell which harness that is, ask the user.

## 2. Find the files for each harness

`~` is the home folder of the user. On Windows it is `%USERPROFILE%`.

| Harness     | Home folder                            | Global instructions file         | Block content |
| ----------- | -------------------------------------- | -------------------------------- | ------------- |
| Claude Code | `$CLAUDE_CONFIG_DIR`, else `~/.claude` | `<home>/CLAUDE.md`               | Import        |
| Codex CLI   | `$CODEX_HOME`, else `~/.codex`         | See below                        | Full text     |
| Antigravity | `~/.gemini`                            | See below                        | Import        |
| Copilot CLI | `$COPILOT_HOME`, else `~/.copilot`     | `<home>/copilot-instructions.md` | Full text     |

Codex CLI reads `<home>/AGENTS.override.md` if it exists and is not empty,
and ignores `<home>/AGENTS.md` in that case. Use `AGENTS.override.md` if it
is not empty. Otherwise use `AGENTS.md`. If you use `AGENTS.override.md`
and `AGENTS.md` also has the block, replace that block too. Codex CLI reads
`AGENTS.md` again when the override is removed, and the block must not be
out of date then.

Antigravity reads global rules from `~/.gemini/GEMINI.md`,
`~/.gemini/AGENTS.md`, `~/.gemini/config/GEMINI.md`, and
`~/.gemini/config/AGENTS.md`. Use the first of these that exists, in this
order. If none exists, use `~/.gemini/GEMINI.md`. Gemini CLI reads that file
too.

## 3. Copy the preferences file

For the harnesses with "Import" in the table, copy `assets/YZ_PREF.md` to
`YZ_PREF.md` in the folder that holds the global instructions file. Replace
the old copy if there is one. Create the folder if it does not exist.

Do not copy the file for Codex CLI or Copilot CLI. Their block holds the
full text.

## 4. Write the block

The block starts and ends with these marker lines:

```markdown
<!-- BEGIN yz:update-global. The update-global skill replaces this block. Do not edit it. -->
<!-- END yz:update-global -->
```

A line that starts with `<!-- BEGIN yz:update-global` is a BEGIN marker,
whatever text follows it.

Between the markers, put one of these:

- Claude Code: `@YZ_PREF.md`. Claude Code resolves the path from the folder
  of `CLAUDE.md`.
- Antigravity: `@[YZ_PREF](YZ_PREF.md)`. Antigravity puts the text of the
  file in place of this line.
- Codex CLI and Copilot CLI: the full text of `assets/YZ_PREF.md`.

Then change the global instructions file:

- If the file does not exist, create it with only the block.
- If the file has the block, replace everything from the BEGIN marker to the
  END marker, both included, with the new block.
- If the file has no block, put the block at the start of the file, then a
  blank line, then the old text. Text that comes later in the file can then
  refine the preferences.

Never change text outside the block. Keep the line endings of an existing
file.

## 5. Check for an old copy of the preferences

Read the text outside the block. If it repeats rules from `YZ_PREF.md`, for
example an older pasted copy of the preferences, show those lines to the user
and ask whether to delete them. Delete them only if the user agrees. Two
copies of the rules make the global instructions longer, and an old copy can
contradict the new one.

## 6. Check the size

Antigravity cuts each rule file at 24,000 bytes, after it puts in the text of
imported files. If the Antigravity global file plus `YZ_PREF.md` is larger
than that, tell the user.

## 7. Report

For each harness, list the files that you created or changed. Claude Code,
Codex CLI, and Copilot CLI read their global instructions when a session
starts, so tell the user to start a new session for them. Antigravity reads
its rules again before each turn and needs no new session.
