# Library cluster

| ID | P | Needs | Claim | Setup | Steps | Expected | Result |
|---|---|---|---|---|---|---|---|
| L1 | P1 | bun | Slice seals only matching files | imprint checkout | `bun test` grep slice test | only `zzz.ts`; distinct digest | pass |
| L2 | P1 | bun | Empty grep throws | same | open with `NO_SUCH_PLANT` | throws slice matched no source files | pass |
| L3 | P1 | e6 | Stuffed ask misses plant | check-e6.sh | run script | stuffed `NONE` | pass |
| L4 | P1 | e6 | Grep slice cites plant path | check-e6.sh | run script | `web/src/pages/index.astro` | pass |
| L5 | P1 | e6 | Old slice does not know new plant | check-e6.sh | run script | `NONE` | pass |
| L6 | P2 | bun | Promote refuses broken marker | bun test | broken-marker test | promote throws | pass |
| L7 | P2 | bun | Promote then rollback moves pointer | bun test | promote/rollback test | open follows pointer | pass |
| L8 | P3 | code | Library budget 400_000 / skip 128_000 | read git.ts imprint.ts | inspect | those numbers | pass (code) |
