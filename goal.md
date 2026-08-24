# Goal

Describe Imprint from the user's point of view. Read [README.md](README.md) first. Use its coverage table as the work list.

## Source snapshot

- Source repository: `/Users/jcoeyman/cloudflare/imprint`
- Last committed source: `3ce8ead166bdc824c91d1a3c66ff63b9cf94603f`
- Working tree: dirty. Slice `open`/`imprint`, Worker `grep`/`paths`, `.astro` text files, and CLI `--grep`/`--path` are present on disk and on the live Worker, but not on `origin/main`.
- Live Worker: `https://imprint-api.coey.dev` version `c6b93e09-e3c1-4791-a82a-5ddd490026dc`
- How to run locally: `cd /Users/jcoeyman/cloudflare/imprint && bun test` and `bun src/cli.ts <command>`
- How to run the Worker: `GET`/`POST` `https://imprint-api.coey.dev`

Every footer cites the commit above and says the working-tree slice work is also required.

## Reading order

1. `src/types.ts` — release, slice, answer.
2. `src/git.ts` — resolve, list, grep, size and extension filters.
3. `src/imprint.ts` — imprint, open, promote, rollback, diff, ask.
4. `src/cli.ts` — command surface.
5. `src/worker.ts` — public ask.
6. `test/imprint.test.ts` — executable behavior.
7. Live `GET https://imprint-api.coey.dev/` and unauthenticated `POST`.

## Writing rules

- Describe the experience, not the implementation.
- Use direct, concrete sentences. Do not hedge or market.
- Put necessary implementation detail in a `Technical note:` block quote.
- Use sentence case headings.
- State surprising behavior plainly.
- Link to the document that owns a shared fact.
- Use terms from [glossary.md](glossary.md).
- Record uncertainty under Open questions. Never guess.
- Do not modify `/Users/jcoeyman/cloudflare/imprint` while drafting.

## Established facts

- A release is bound to one commit and one slice key. Full tree uses key `full`. A grep or path list uses a 16-hex digest of that request.
- Library default budget is 400_000 bytes. Files larger than 128_000 bytes are skipped. Text extensions include `.astro`.
- Alphabetical A–Z fill: files are sorted by path, then taken until the budget is full. A plant later in the alphabet can be absent from a stuffed snapshot.
- `open` without a slice reuses `.imprint/releases/<commit>.json` if present; otherwise it imprints.
- `open` with a slice reuses `.imprint/releases/<commit>.<sliceKey>.json` if present.
- Empty grep or path slice throws: `commit <sha> slice matched no source files`.
- Ask citations are file paths that appear as substrings in the model text and exist in the sealed files.
- Promote and rollback only move the **full** release pointer. They ignore slices.
- Promote refuses a release whose `proof.verified` is false. A file containing `IMPRINT_BROKEN` fails verification.
- Rollback is promote of the named commit.
- CLI ask needs `AI_GATEWAY_URL` and `AI_GATEWAY_TOKEN` (or `CF_ACCESS_TOKEN`). There is no default employee gateway hostname.
- Worker GET is public usage JSON. Worker POST ask requires `Authorization: Bearer $ASK_API_TOKEN`. Missing token config is 503. Wrong bearer is 401. `DEV_MODE=true` skips the gate.
- Worker commit must be a hex SHA (7–40). Repository must be `owner/name` or a public GitHub URL.
- Worker question max 2_000 characters. Body max 16_000 bytes. Grep max 200 characters. At most 40 files and 300_000 bytes. Grep scans at most 200 candidates.
- Worker refuses a model name that matches `/qwen/i`.
- Live check 2026-08-24: GET 200 usage JSON including `grep` and `paths`. POST without bearer 401 `unauthorized`. POST with a bad bearer 401 `unauthorized`.

## Order of work

1. Scaffold.
2. Pilot `library/ask-on-a-slice.md`.
3. Write the two foundation documents.
4. Draft remaining features.
5. Verification checklists and bug triage.
