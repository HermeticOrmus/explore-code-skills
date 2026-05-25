# Using this repo with Cursor

This project includes a Cursor project rule so the exploration protocol is available when you work here.

## In this repository

1. Open the folder in Cursor.
2. The rule [`.cursor/rules/explore-code.mdc`](.cursor/rules/explore-code.mdc) is committed with `alwaysApply: false`, so it applies situationally. When you ask Cursor to explore or orient in a codebase, reference it by name.
3. In Cursor, confirm under **Settings → Rules**, where `explore-code` should appear.

## Use the same protocol in another project

**Cursor (recommended)**: Copy `.cursor/rules/explore-code.mdc` into that project's `.cursor/rules/` directory (create the folders if needed). Because the rule is situational rather than always-on, invoke it when you land in unfamiliar code.

**Other AI coding tools**: If a stack only supports a root instruction file, copy [`CLAUDE.md`](CLAUDE.md) into that project instead (or merge its contents into your existing instructions). Most modern AI coding tools (Claude Code, Continue, Cline, Windsurf, Aider) read a root-level instruction file.

## Optional: personal Agent Skills

If you want the same content as a reusable skill under `~/.cursor/skills`, use [`skills/explore-code/SKILL.md`](skills/explore-code/SKILL.md). Copy or symlink it into your personal skills directory.
