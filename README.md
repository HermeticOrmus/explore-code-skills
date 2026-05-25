<p align="center">
  <img src="https://ormus.solutions/mascot/pixellab_liquid_to_compass.gif" alt="Explore Code Skills" width="128" style="image-rendering: pixelated;" />
</p>

<h1 align="center">Explore Code Skills</h1>

<p align="center">
  <em>A Claude Code skill for understanding an unfamiliar codebase fast — systematic exploration from entry points to data flow.</em>
</p>

<p align="center">
  <a href="https://github.com/HermeticOrmus/explore-code-skills/stargazers"><img src="https://img.shields.io/github/stars/HermeticOrmus/explore-code-skills?style=flat-square&color=aa8142" alt="Stars" /></a>
  <a href="https://github.com/HermeticOrmus/explore-code-skills/blob/main/LICENSE"><img src="https://img.shields.io/github/license/HermeticOrmus/explore-code-skills?style=flat-square&color=aa8142" alt="License" /></a>
  <a href="https://github.com/HermeticOrmus/explore-code-skills/commits"><img src="https://img.shields.io/github/last-commit/HermeticOrmus/explore-code-skills?style=flat-square&color=aa8142" alt="Last Commit" /></a>
  <img src="https://img.shields.io/badge/Claude_Code-aa8142?style=flat-square&logo=anthropic&logoColor=white" alt="Claude Code" />
</p>

---

## The problem

You clone a repository you have never seen. Hundreds of files, a framework you half-know, no architecture diagram, and a task that assumes you already understand the place. The instinct is to grep for a keyword and start reading whatever turns up, but reading files in random order builds no model. An hour later you know some functions and still cannot answer where a request enters or where your change belongs.

The skill is orientation: building a working mental model of an unfamiliar codebase before touching it. Not a full audit, just enough to act in the right place instead of the first place that compiles.

## The exploration

Four steps, in order:

1. **Start with purpose, architecture, stack, entry points.** Answer four questions from the README and the manifest before reading source. What it does, what shape it is, what it is built on, where execution starts.
2. **Map the directory structure.** Get the tree, ignoring generated directories. Locate source, tests, config, docs, and build definitions. The layout encodes the team's mental model.
3. **Trace one representative flow end to end.** Follow one request, one command, or one public call through every layer. One traced flow teaches more than many files read in isolation.
4. **Identify the core abstractions and where to make changes.** Name the data models, the seams between layers, the cross-cutting concerns, and the files most changes touch. Then answer the orientation questions that turn a map into the ability to act.

Full protocol: [`CLAUDE.md`](CLAUDE.md). Worked walkthroughs: [`EXAMPLES.md`](EXAMPLES.md).

## Install

### As a project CLAUDE.md

Drop [`CLAUDE.md`](CLAUDE.md) at the root of your repository. Claude Code picks it up automatically. Merge with existing project instructions if any.

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/HermeticOrmus/explore-code-skills/main/CLAUDE.md
```

### As a Claude Code skill

The same content is packaged as a skill under [`skills/explore-code/`](skills/explore-code/) for `~/.claude/skills/`. See the `SKILL.md` inside for installation.

### As a slash command

Save the protocol as a command so you can invoke it on demand:

```bash
curl -o ~/.claude/commands/explore-code.md https://raw.githubusercontent.com/HermeticOrmus/explore-code-skills/main/CLAUDE.md
```

Then run `/explore-code` in any Claude Code session to orient in the current repository.

### In Cursor

See [`CURSOR.md`](CURSOR.md) for the Cursor-rule equivalent at [`.cursor/rules/explore-code.mdc`](.cursor/rules/explore-code.mdc).

### In other AI coding tools

If your tool reads a single instruction file at the project root, copy `CLAUDE.md` to whatever name your tool expects (`AGENTS.md`, `INSTRUCTIONS.md`, etc.).

## See also

- [`hypothesis-debugging-skills`](https://github.com/HermeticOrmus/hypothesis-debugging-skills): the companion for once you find the bug, covering hypothesis-driven debugging instead of trial and error.

## Contributing

PRs welcome, especially for additional worked walkthroughs in [`EXAMPLES.md`](EXAMPLES.md), translations of the README, and adaptations of `CURSOR.md` for other AI coding tools (Windsurf, Cline, Aider, Continue, etc.).

## License

MIT. Use it, fork it, merge it into your own CLAUDE.md.
