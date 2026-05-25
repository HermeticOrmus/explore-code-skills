# Examples

Two worked walkthroughs of the exploration protocol. Each shows the questions asked and the order they were asked in, so the order itself becomes the lesson.

---

## 1. Exploring a web backend you have never seen

The task: add a field to the order-creation API in a Node and TypeScript service you have never opened.

### Step 1: purpose, architecture, stack, entry points

Read the README and `package.json` first.

```bash
cat README.md
cat package.json
```

Answers gathered:

- **Purpose**: REST API for an order-management system.
- **Architecture**: single service, Express, talking to Postgres.
- **Stack**: Node 20, TypeScript, Express 4, Prisma, Jest. Lockfile confirms versions.
- **Entry point**: `package.json` `start` script runs `dist/server.js`, which compiles from `src/server.ts`.

Open the entry point and read it top to bottom.

```bash
grep -n "listen\|app.use\|router" src/server.ts
```

It registers middleware, mounts routers from `src/routes/`, and calls `app.listen`. Now the shape is known.

### Step 2: map the directory structure

```bash
tree -L 2 -I 'node_modules|dist|.git'
```

The tree shows `src/routes/`, `src/services/`, `src/repositories/`, `src/models/`, and `tests/`. The three-layer naming (routes, services, repositories) signals the layering convention before any handler is read.

### Step 3: trace one representative flow end to end

Order creation is the representative flow. Trace it in layer order.

```bash
grep -rn "orders" src/routes/
```

- `src/routes/orders.ts` registers `POST /orders` and calls `OrderService.create`.
- `src/services/orderService.ts` validates input, then calls `OrderRepository.insert`.
- `src/repositories/orderRepository.ts` runs the Prisma `order.create` call.
- The response shape is built back in the service and returned by the route.

That single trace reveals the convention: route validates the HTTP envelope, service holds business rules, repository owns persistence. Every other endpoint is a variation on this.

### Step 4: core abstractions and where to make changes

- **Data model**: the `Order` model in `prisma/schema.prisma`.
- **Seams**: route to service to repository.
- **Cross-cutting**: validation lives in the service, auth in middleware mounted in `server.ts`.

Now the orientation question answers itself: to add a field to order creation, edit the Prisma schema and migrate, extend the validation in `orderService.ts`, and pass it through `orderRepository.ts`. The route changes only if the field is client-supplied. The change has a known home before a line is written.

---

## 2. Orienting in a large monorepo

The task: understand where a shared authentication helper lives across a monorepo with sixteen apps, so a fix can be made once instead of sixteen times.

### Step 1: purpose, architecture, stack, entry points

In a monorepo the entry point is the workspace definition, not a single `main`.

```bash
cat package.json
cat pnpm-workspace.yaml
```

Answers gathered:

- **Purpose**: a command-center monorepo bundling many internal apps.
- **Architecture**: workspaces, with `apps/*` for deployables and `packages/*` for shared code.
- **Stack**: pnpm workspaces, TypeScript, a mix of Next.js apps.
- **Entry points**: each app under `apps/` has its own entry; shared logic is in `packages/`.

In a monorepo, "entry point" is plural. The first orientation move is to separate the deployables from the shared libraries.

### Step 2: map the directory structure

```bash
tree -L 2 apps packages
```

The tree shows sixteen directories under `apps/` and a smaller set under `packages/`. The shared helper, if it exists, is in `packages/`. This narrows a sixteen-app search to a handful of library directories.

### Step 3: trace one representative flow end to end

Instead of tracing a request, trace a dependency: where does authentication come from, and who imports it.

```bash
grep -rn "from '@workspace/auth'" apps/ packages/
grep -rn "verifyToken\|getSession\|requireAuth" packages/
```

The grep shows every app importing `@workspace/auth`, and the implementation lives in `packages/auth/src/`. Tracing the import graph, rather than one runtime flow, is the right representative trace for a monorepo: it reveals which code is shared and which is per-app.

### Step 4: core abstractions and where to make changes

- **Shared abstraction**: `packages/auth` is the single source for session and token logic.
- **Seam**: apps depend on the package; the package depends on nothing app-specific.
- **Where the change goes**: editing `packages/auth/src/` fixes all sixteen apps at once, provided each app pins the workspace version rather than vendoring a copy.

The orientation question (fix once or sixteen times) is answered by the import graph. Because every app imports the same package, the fix has one home. Had the grep shown apps with their own copied auth code, the answer would have been different, and the exploration would have caught it before the fix went in the wrong place.

---

## A note on order

The steps are ordered on purpose. Framing before structure, structure before tracing, tracing before naming abstractions. Skipping ahead, such as grepping for a symbol before knowing the stack or editing before tracing a flow, is how an hour disappears into files that build no model. The order is the discipline.
