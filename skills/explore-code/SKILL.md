---
name: explore-code
description: Understand an unfamiliar codebase fast. Start with purpose, architecture, stack and entry points, map the directory structure, trace one representative flow end to end, then identify the core abstractions and where changes go. Use when landing in a repository you have never seen and need orientation before making a change.
license: MIT
---

# Explore code

A protocol for building a working mental model of an unfamiliar codebase before changing it. The goal is orientation, not a full audit: enough understanding to know where execution starts, how data moves, and where a given change belongs.

**Tradeoff**: bias toward building a map before editing. For a one-line fix in a file you already know, skip it.

## 1. Start with purpose, architecture, stack, entry points

Before reading source, answer four questions from the README and the manifest:

- Purpose: what does it do, and for whom?
- Architecture: single service, library, CLI, monorepo, frontend plus backend?
- Stack: language, framework, runtime, datastore, and versions.
- Entry points: where execution starts (a `main`, a server `listen`, a CLI registration, a framework convention).

## 2. Map the directory structure

Get the tree, ignoring generated and vendored directories.

```bash
tree -L 2 -I 'node_modules|dist|build|.git|target|__pycache__|.venv'
```

Locate source versus tests, config and environment templates, docs, and build and deploy definitions. The layout encodes the team's mental model.

## 3. Trace one representative flow end to end

Follow one request, one command, or one public call through every layer: route to handler to service to data access to response, or argument parsing to side effect. One traced flow teaches more than many files read in isolation, because it reveals the layering and the conventions repeated everywhere else.

## 4. Identify the core abstractions and where to make changes

Name the data models, the seams between layers, the cross-cutting concerns, and the files most changes touch. Then answer the orientation questions:

1. How would I add a new feature of type X?
2. Where would I look to fix a bug in area Y?
3. What happens when event Z occurs?
4. How does component A interact with component B?

When you can answer these, you can change the right place instead of the first place that compiles. Produce a short orientation note (the four framing answers, the one flow traced, the core abstractions, and open questions for maintainers) as the artifact.

---

See full content at https://github.com/HermeticOrmus/code-comprehension-skills.
