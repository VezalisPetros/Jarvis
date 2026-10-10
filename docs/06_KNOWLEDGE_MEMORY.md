# Business knowledge and memory

Agreed: persistent business context with correction, deletion and source-grounded answers. Proposed data semantics below. Development memory in `project/` is not business memory.

## Information classes

| Class | Meaning | Can it authorize action? |
|---|---|---|
| Fact | Source-backed or owner-confirmed business statement | No; facts inform work, policy grants authorize it |
| Hypothesis | Model interpretation or unconfirmed owner implication | No; label explicitly and ask targeted clarification |
| Decision | Explicit owner choice with scope and effective date | Only through a separately validated grant when needed |
| Source | Versioned supporting document/record | No |
| Goal | Desired outcome and acceptance criteria | Only within existing permissions |
| Episode | Prior run summary with artifact/evidence links | No; success once does not imply permission forever |

A claim record has claim_id, workspace_id, kind, text, source_refs, status, valid_from, valid_until if known, confirmed_by, confirmed_at, supersedes_id and created_at. Unknown timestamps stay null. Model confidence is not confirmation. Example: “parking is available” without explicit instructions is a hypothesis, not a fact.

## Ingestion

First fixture step uses plain text with stable source IDs and line locators. Later uploads accept TXT, CSV, DOCX and text-extractable PDF as proposed in the earlier PRD. Scan-only PDFs return unsupported_content with a helpful explanation; no silent empty index. Parse with type/size limits, reject path traversal, preserve locators and record source version/checksum. Store chunks as derived data.

Default later upload limits: 10 MB per file and 20 active source files per workspace. Enforce on server as well as client. These are engineering guardrails, not commercial limits. Raw source bytes and extracted text share an access policy. A parsing failure does not mark a source ready.

## Updating and conflicting evidence

An owner correction creates a new claim/source version and marks the old one superseded. Preserve metadata history, but select only current valid evidence by default. If sources disagree and precedence has not been explicitly resolved, return the conflict and ask the owner rather than choose whichever snippet ranks highest.

Runs pin a source snapshot for reproducibility. Before emitting a result, check whether referenced sources were corrected, expired or deleted. A corrected source makes a dependent artifact `stale`; it can remain visible as historical with a clear label. Do not silently present it as current or rewrite a completed artifact without a new version.

## Deletion

Deletion first revokes retrieval/access to the source transactionally, then removes source content, chunks/embeddings and cache entries, and redacts or removes copied source text from dependent artifacts/traces according to the chosen retention policy. Mark affected artifacts withdrawn if they can no longer be safely shown. Stop new calls in affected runs; request replacement input. Retain only minimal tombstone metadata necessary for audit, without deleted content.

Default target from prior PRD: active-store deletion completed within 24 hours, with immediate exclusion from reads. Backup expiry is an open cloud decision before storing real data. A restore must reapply deletion tombstones before serving traffic. Synthetic-only early development does not require collecting personal data to test this.

## Retrieval and context assembly

Select workspace-scoped active sources, then retrieve. Include exact source/version/locator citations; do not attach a plausible citation after unsupported generation. Keep confirmed facts, hypotheses and decisions separately labeled in model context. A compressed memory summary links to its source records and is invalidated by corrections. Truncation must not remove safety-critical uncertainty or imply complete evidence.

## Acceptance

Correct a parking instruction and verify new runs use the updated version. Introduce two conflicting policies and verify a clarification request. Delete a source and verify it disappears from lexical/vector retrieval, direct download and cached answers. Attempt access from a second workspace and receive no content. No current learning, customer outcome or deployment state may be inferred from runtime memory.
