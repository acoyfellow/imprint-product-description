# Imprint product description

A written description of Imprint as the user meets it: seal a Git commit (or a slice of it), ask a question against that seal, and get an answer that can only cite files that were sealed.

## Purpose

Imprint binds selected source files to an exact Git commit, records digests, and optionally sends that snapshot to a model. This repository describes that behavior from the outside in.

The surface is the **library and CLI on a local Git checkout**, plus the **public Worker ask** at `https://imprint-api.coey.dev`. Defaults: library `maxSourceBytes` 400_000; Worker 40 files / 300 KB; Worker ask fail-closed on a bearer token.

### What this is not

- Not API documentation. Types remain in the Imprint source.
- Not organized by source package. A behavior appears once, where the user meets it.
- Not a description of Cloudbox, Code2LoRA adapters, or the marketing site at imprint.coey.dev.
- Not a promise that the local Git tree on `main` matches the live Worker. See [goal.md](goal.md).

## Interaction model

### Unit and phases

The unit is a **command**.

1. **Starting.** The user invokes `imprint`, `open`, `ask`, `promote`, `rollback`, `diff`, `inspect`, or `prove`, or POSTs to the Worker.
2. **Ending at once.** Missing inputs, a refused gate, or an empty slice stop before any model call. The process prints an error and exits 1, or the Worker returns 4xx/5xx.
3. **Becoming extended.** Git resolve, file read, or a model request has started.
4. **While extended.** The user sees no progress stream. The command holds the terminal or HTTP connection until it finishes or fails.
5. **Finishing.** JSON is printed or returned. A new or reused release file may exist under `.imprint/releases/`.

### Variant axis

The same command changes by: commit ref; full tree vs slice (`grep` / `paths`); whether a release already exists; whether a model provider is configured; Worker vs local; `DEV_MODE`; body size; file type and size.

### Interrupt list

Every interactive feature answers these rows in this order:

1. **Explicit abort.** The user sends SIGINT / closes the HTTP client.
2. **Different action.** The user starts another command on the same checkout.
3. **Environment failure.** Git, disk, network, GitHub, or the model fails.
4. **Target changes.** The working tree or remote commit changes while the command runs.
5. **Input channel changes.** stdin/TTY or HTTP connection drops.

### Cross-cutting order

permissions; history; containers and runtime; locked or read-only state; offline; collaboration; notifications; preferences.

## Document skeleton

Each feature document uses the same eight sections listed in [goal.md](goal.md).

## Structure

```text
README.md
AGENTS.md
CLAUDE.md
goal.md
glossary.md
bug-triage.md

foundations/
  command-lifecycle.md     invoke, refuse, wait, finish
  release-and-slice.md     commit, files, digest, full vs slice

library/
  imprint-a-commit.md      imprint / open
  ask-on-a-slice.md        ask against a sealed snapshot (pilot)
  promote-and-rollback.md  active pointer
  inspect-and-diff.md      inspect and diff

worker/
  public-ask.md            GET usage and POST ask

verification/
  README.md
  library.md
  worker.md
```

## Coverage

| Document | Status |
|---|---|
| foundations/command-lifecycle.md | drafted |
| foundations/release-and-slice.md | drafted |
| library/imprint-a-commit.md | drafted |
| library/ask-on-a-slice.md | drafted |
| library/promote-and-rollback.md | drafted |
| library/inspect-and-diff.md | drafted |
| worker/public-ask.md | drafted |
| verification/library.md | drafted |
| verification/worker.md | drafted |

A document becomes `verified` only when a person has run its P1 and P2 rows.

## Source map

- Interaction: `src/cli.ts`, `src/imprint.ts`, `src/worker.ts`
- Thresholds: `src/git.ts`, `src/worker.ts`, `src/imprint.ts`
- Tests: `test/imprint.test.ts`, `test/adapter-service.test.ts`
- Live Worker: `https://imprint-api.coey.dev`
