# Shared data and API contracts

These are proposed internal application contracts. `schema_version: 1` describes this package's data model, not an external API version. Implement strict typed validation later; JSON examples are not executable validators.

## Shared rules

IDs are opaque server-issued strings. Examples use visibly synthetic IDs. Timestamps use ISO 8601 UTC; user schedules also store an IANA timezone. Money uses integer minor units with explicit currency or decimal strings with documented precision; never binary floating point for budget enforcement. Unknown usage/cost is null. Runtime attempts, source versions and immutable plan versions are distinguishable.

Every tenant-owned row has workspace_id. Add composite references or equivalent database constraints so a cross-workspace artifact cannot be attached to a run. Versioned records are append-only plus lifecycle metadata. Current projections may update with optimistic concurrency; history remains traceable.

## Core records and uniqueness

| Record | Required fields / constraint |
|---|---|
| Workspace membership | actor_id, workspace_id, role; unique pair |
| KnowledgeSource | source_id, version, workspace_id, status, object_ref, checksum, locators |
| MemoryClaim | claim_id, kind, status, provenance, supersedes_id; see memory spec |
| PlanVersion | plan_id, version, workspace_id, immutable payload, payload_hash, validation |
| AgentConfigVersion | agent_id, version, workspace_id, tool/source/output scopes |
| Run | run_id, workspace_id, plan_ref, status, version, request_id, limits, timestamps |
| StepAttempt | run_id, step_id, attempt_no, status, lease_generation, lease_expiry |
| ToolCall | call_id, logical_key, input_hash, tool_version, status, usage, artifact_refs |
| Approval | exact payload hash, actor, expiry, status; see security spec |
| Artifact | artifact_id, workspace_id, contract, version, source_refs, status, object_ref |
| BudgetReservation | reservation_id, run_id, call_id, amount, state; unique call reservation |
| Event | workspace_id, run_id, sequence, type, time, sanitized payload; unique run/sequence |

Uniqueness rules: workspace/request_id for run creation; workspace/logical_key for idempotent writes; schedule/version/period for briefings. Reusing a request key with changed payload returns conflict.

## Output contracts

`grounded_answer.v1`: question, status (`answered`, `needs_information`, `conflicting_evidence`), answer (nullable), claims (text with source refs), missing_information list and warnings. An answered factual claim requires at least one resolvable supporting citation. Empty answers cannot be marked answered.

`question_groups.v1`: dataset_ref, assignments of record_id to one primary category, excluded_ids with reasons. No invented record IDs. The aggregation tool performs counting.

`analysis_report.v1`: dataset_ref, category counts/denominator, draft_refs, unresolved_questions, source_snapshot and coverage. Each required section must exist before completion. Missing drafts are shown as gaps.

`review_result.v1`: artifact_refs, passed boolean, checks with outcomes and reasons. Reviewer assertions are evidence for a check, not permission to bypass backend validation.

## Proposed endpoints and semantics

Endpoint paths are internal design defaults, not implemented routes.

| Endpoint | Behavior |
|---|---|
| POST /runs | Validates request_id/objective/source refs; returns run_id and durable status after commit |
| GET /runs/{id} | Authorized snapshot with version, steps, artifacts, warnings and usage |
| GET /runs/{id}/events | Optional resumable stream; event replay cannot execute actions |
| POST /runs/{id}/pause | Idempotent pause request; returns actual state |
| POST /runs/{id}/resume | Revalidates current permissions/sources/plan; requires expected_version |
| POST /runs/{id}/cancel | Records cancellation and returns cancelling or cancelled |
| POST /approvals/{id}/decision | Authenticated grant/deny with expected payload hash/version |
| POST /sources | Bounded upload, versioned metadata and asynchronous parsing state |
| DELETE /sources/{id} | Revokes active access and initiates deletion propagation |
| PATCH /memory/{id} | Creates a corrected claim version with expected_version |
| GET /artifacts/{id} | Scope-checked artifact and citation metadata |

Typical status semantics: 202 for accepted unfinished work, 200 for existing result, 400 for invalid input, 401 unauthenticated, 403 for a known operation without permission, 404 for non-disclosing object lookup, 409 for version/idempotency conflict, 429 for rate limit, 503 for dependency unavailable. The domain error code provides detail; HTTP status alone does not define run success.

## Evidence and event example

`step.failed` includes run_id, step_id, attempt_no, tool ID if relevant, sanitized error code and retry decision. It excludes raw secrets and hidden model reasoning. A current snapshot is authoritative if event delivery is delayed. Persist state and corresponding events transactionally so the UI cannot receive a completion for a missing artifact.

Read [examples index](../examples/README.md) for illustrative objects. Validate them against executable schemas only after those schemas have been implemented; package JSON parsing checks do not prove runtime contract enforcement.
