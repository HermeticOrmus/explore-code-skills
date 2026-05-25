# CLAUDE.md

A protocol for understanding an unfamiliar codebase fast. Merge with project-specific instructions as needed.

Use this when you land in a repository you have never seen and need a working mental model before you change anything. The goal is orientation, not a full audit: enough understanding to know where execution starts, how data moves, and where a given change belongs.

**Tradeoff**: This biases toward building a map before editing. For a one-line fix in a file you already know, skip it.

## 1. Start with purpose, architecture, stack, entry points

Before reading any source, answer four questions:

- **Purpose**: What does this project do, and for whom? The README, the package description, and the top-level directory names usually answer this in under a minute.
- **Architecture**: Is it a single service, a library, a CLI, a monorepo, a frontend plus backend? The shape determines where to look next.
- **Stack**: What language, framework, runtime, and datastore? Read the manifest (`package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`, `pom.xml`) and the lockfile to confirm versions.
- **Entry points**: Where does execution start? A `main` function, a server `listen` call, a CLI command registration, an `index` module, or a framework convention (`app/`, `pages/`, `cmd/`).

Write these four down before going deeper. They are the frame everything else hangs on.

## 2. Map the directory structure

Get the shape of the tree, ignoring generated and vendored directories.

```bash
tree -L 2 -I 'node_modules|dist|build|.git|target|__pycache__|.venv'
```

From the tree, locate:

- Where source lives versus where tests live
- Configuration files and environment templates (`.env.example`, `config/`)
- Documentation (`README`, `docs/`, `ARCHITECTURE.md`, `CONTRIBUTING`)
- Build and deploy definitions (`Dockerfile`, CI workflows, `Makefile`)

The directory layout encodes the team's mental model. Naming conventions and folder boundaries tell you how the maintainers think about the system.

## 3. Trace one representative flow end to end

Pick the most representative thing the system does and follow it through every layer. For a web backend, trace one request: route registration to handler to service to data access to response. For a CLI, trace one command from argument parsing to side effect. For a library, trace one public function from the exported surface to the internal call it delegates to.

```bash
grep -rn "router\|app.get\|app.post\|@app.route\|addEventListener" --include='*.ts' src/
```

Tracing a single flow teaches more than reading many files in isolation. It shows the layering, the abstractions that sit between layers, and the conventions the codebase repeats everywhere else. Once you have traced one flow, the rest of the system tends to be variations on it.

## 4. Identify the core abstractions and where to make changes

After tracing one flow, name the abstractions the codebase is built on:

- Data models and their schema (database tables, types, domain objects)
- The seams between layers (controllers, services, repositories, or whatever the project calls them)
- Cross-cutting concerns (authentication, configuration, logging, error handling)
- The handful of files that almost every change touches

Then answer the orientation questions that turn a map into the ability to act:

1. How would I add a new feature of type X? (Name the files you would create or edit.)
2. Where would I look to fix a bug in area Y?
3. What happens when event Z occurs? (Trace it.)
4. How does component A interact with component B?
5. Why was a given decision made this way? (Note questions for maintainers rather than guessing.)

When you can answer these, you have enough to make a change in the right place instead of the first place that compiles.

---

## Output of an exploration

Produce a short orientation note, not a textbook:

- The four framing answers (purpose, architecture, stack, entry points)
- The one flow you traced, in layer order
- The core abstractions and the files most changes touch
- Open questions for the maintainers

This note is the artifact. It is what you would hand to the next person landing in the same codebase.

---

**Companion**: Once exploration finds the bug, switch to hypothesis-driven debugging. See [`hypothesis-debugging-skills`](https://github.com/HermeticOrmus/hypothesis-debugging-skills).

**License**: MIT. Use it, fork it, merge it into your own.
