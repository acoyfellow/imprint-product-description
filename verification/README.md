# How to verify

1. Confirm source: `git -C /Users/jcoeyman/cloudflare/imprint rev-parse HEAD` is `3ce8ead166bdc824c91d1a3c66ff63b9cf94603f` and the working tree still has slice `open`.
2. Run P1, then P2, then P3.
3. Record `pass` / `fail` / `blocked`.
4. File every fail in [../bug-triage.md](../bug-triage.md). A fail may mean the document is wrong.

Local: `cd /Users/jcoeyman/cloudflare/imprint && bun test`.

Live Worker: `curl -sS https://imprint-api.coey.dev/` and a POST without a bearer.

This pass ran GET, unauthenticated POST, `bun test`, and `check-e6.sh`. It did not run an authenticated Worker ask.
