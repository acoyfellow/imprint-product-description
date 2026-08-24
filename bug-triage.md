# Bug triage

Suspected defects from the description pass. Deduplicated by root cause.

| Severity | Item | Decision |
|---|---|---|
| medium | Promote status is not unique | product call |
| medium | CLI cannot imprint a slice | product call |
| medium | Worker slice is not a stored release | product call |
| medium | Worker grep only scans 200 A–Z files | product call |
| low | CLI constructs a gateway client before every command | fix |
| low | Guardrail treats public GET usage as OPEN | product call |
| low | Citation = substring of model text | product call |

## Promote status is not unique

**Where.** After promote then rollback, both release JSON files can say `status: active`.

**Happens.** `open()` follows `.imprint/active`, so behavior is right. Inspect of the old file still says active.

**Cause.** `promote` in `src/imprint.ts` sets the chosen release to active and never writes the previous one back to candidate.

**Severity.** Medium. Recoverable.

**Raised by.** [library/promote-and-rollback.md](library/promote-and-rollback.md)

## CLI cannot imprint a slice

**Where.** `imprint imprint` has no `--grep` / `--path`.

**Happens.** User must ask or use the library to seal a slice.

**Cause.** `src/cli.ts` only parses those flags on `ask`.

**Severity.** Medium.

**Raised by.** [library/imprint-a-commit.md](library/imprint-a-commit.md)

## Worker slice is not a stored release

**Where.** POST ask.

**Happens.** Caller gets `selectedFiles` but no `sourceDigest` to pin.

**Cause.** `src/worker.ts` builds context in memory.

**Severity.** Medium. The library pin exists; the public Worker does not share it.

**Raised by.** [worker/public-ask.md](worker/public-ask.md)

## Worker grep only scans 200 A–Z files

**Where.** POST with `grep` on a large repo.

**Happens.** A match after the 200th text path can be omitted.

**Cause.** `maxGrepScan = 200` in `src/worker.ts`.

**Severity.** Medium.

**Raised by.** [foundations/release-and-slice.md](foundations/release-and-slice.md)

## CLI constructs a gateway client before every command

**Where.** `imprint imprint` with no gateway env.

**Happens.** Process throws `ask needs AI_GATEWAY_URL…` even for non-ask commands.

**Cause.** `const imprint = client()` at module scope in `src/cli.ts`.

**Severity.** Low. Looks like a bug.

**Raised by.** [foundations/command-lifecycle.md](foundations/command-lifecycle.md)

## Guardrail treats public GET usage as OPEN

**Where.** After `wrangler deploy` of imprint.

**Happens.** Deploy succeeds; guardrail exits 2 because GET 200 has no auth. POST is 401.

**Cause.** Guardrail probes GET `/`.

**Severity.** Low. Documented as intentional usage.

**Raised by.** [worker/public-ask.md](worker/public-ask.md)

## Citation is a substring

**Where.** Library ask JSON `citations`.

**Happens.** Any sealed path that appears in the answer text is listed.

**Cause.** `citations()` in `src/imprint.ts`.

**Severity.** Low.

**Raised by.** [library/ask-on-a-slice.md](library/ask-on-a-slice.md)
