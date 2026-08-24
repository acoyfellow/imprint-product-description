# Glossary

**Active pointer.** The commit written to `.imprint/active`. `open()` with no ref uses this, then HEAD.

**Ask.** Send a question to a model together with one sealed snapshot. The model is told to use only that snapshot.

**Candidate.** A release that has been written but not promoted.

**Citation.** A sealed file path that also appears as a substring in the model answer.

**Command.** One CLI invocation or one Worker request.

**Commit.** A resolved Git object id. The user may pass `HEAD` or another ref locally. The Worker accepts only a hex SHA.

**Digest.** SHA-256 hex of the sealed manifest (and, separately, of the concatenated snapshot text).

**Extended.** The command has started Git, disk, network, or a model call and has not yet printed a result.

**Full snapshot.** A release with slice key `full`: A–Z text files until the byte budget.

**Grep.** A literal string. Locally, `git grep -F` at that commit. On the Worker, a substring test of fetched file text.

**Imprint (verb).** Resolve a commit, select files, write a release JSON, return it.

**Open.** Load a release for a commit and slice, or imprint if the file is missing.

**Promote.** Mark a verified full release active and write the active pointer.

**Proof.** The list of checks on a release. `verified` is true only if every check passed.

**Release.** The JSON record of one commit plus one slice: files, digests, checks, status.

**Rollback.** Promote a different already-imprinted full commit.

**Seal.** The act of choosing files and recording their bytes and digest so later asks use that exact set.

**Slice.** An optional path list or grep that restricts which files are sealed.

**Slice key.** `full`, or the first 16 hex characters of the SHA-256 of `{paths, grep}`.

**Stuffed snapshot.** A full snapshot that filled the byte budget before later paths.

**Worker ask.** `POST https://imprint-api.coey.dev` with repository, commit, question, and optional grep or paths.
