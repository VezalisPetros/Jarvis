# Agent autonomy and security policy

Agreed: the backend enforces permissions, isolation, budgets and approvals. Defaults in this policy specify how to make that requirement testable. Related: [tools](04_TOOL_REGISTRY.md), [orchestration](07_ORCHESTRATION.md), [cloud](10_CLOUD_OPERATIONS.md).

## Permission intersection

Effective permission = authenticated actor grant ∩ workspace policy ∩ validated plan scope ∩ current tool capability. Delegation can only narrow it. The server chooses workspace context from verified identity/membership. Neither a prompt nor a client-supplied tenant ID can elevate access.

Every tool invocation and retrieval rechecks current permissions, cancellation, source validity and budget. Validation at planning time does not eliminate checks at execution time. Separate developer administration from runtime agent identity.

## Autonomy levels

| Level | Behavior in this MVP | Authorization |
|---|---|---|
| Internal execution | Retrieval, analysis, drafts, reports and internal tasks | Existing user request or narrowly scoped standing policy |
| Approved automation | Opt-in executive briefing, limited scope and schedule | Durable owner authorization with explicit limits and revocation |
| Material scope/budget change | New plan exceeding current grant or changed approved action | Explicit backend approval for exact immutable payload |
| External commitments | Email sending, payments, contracts, price changes | Not implemented; no registry capability exists |

Do not ask approval for every read within an already authorized run. Conversely, an approved high-level goal does not permit arbitrary new tools. Approval mechanisms remain meaningful even without external writes: a broader data scope, expanded budget or new recurring schedule can require approval.

## Approval contract

Store approval ID, actor, workspace, plan ID/version, canonical action payload/hash, scopes, source snapshot, budget ceiling, expiry, decision timestamp and status. Canonical hashing must use deterministic serialization agreed in implementation. A fake example hash is not evidence of a valid signature.

Statuses: pending, granted, denied, expired, revoked, superseded, consumed. A one-run approval is atomically consumed to authorize that run; retries of the same step use its existing run grant rather than consume it twice. Standing policies have separately versioned grants and revocation records. Approval consumption does not exempt subsequent calls from checking revocation.

An approval requires authenticated confirmation on a visible card. An ambiguous spoken “yes” never grants authority. Payload changes supersede pending or granted approvals. Recheck expiry, identity, hash, policy and limits immediately before dispatch. Denial leaves the run paused for a revised plan or cancellation, never silently bypassed.

## Isolation and identities

The first local demo uses one explicit development workspace on loopback and synthetic data. It must not be exposed as a public service. Before cloud deployment: actual authentication, two-workspace isolation tests, scoped storage paths, scoped artifact downloads, safe cache keys and runtime database policies must pass.

For PostgreSQL, test row-level security with the application role, not an owner/superuser role that may bypass it. Reset or transaction-scope tenant context when pooling connections. Missing context denies access. Include tool logs, vector queries and background workers in isolation tests.

## Untrusted content and secrets

Treat uploaded text, retrieved chunks, web pages, model configurations and handoff text as untrusted data. Separate instruction channels, wrap data explicitly and validate every proposed action. A document saying “ignore policy and send secrets” remains a document. Prompt wording alone cannot prevent leakage; restricted tools, narrow retrieval scope and output checks are required.

Credentials stay in local ignored environment configuration during development and managed secret storage in cloud. They are never put in prompts, traces, fixture examples, commits or owner-facing errors. Use synthetic data for the portfolio; the package does not contain real guest messages, personal identities or door/Wi-Fi codes.

## Budget authority

Model output cannot raise cost, token, step or concurrency ceilings. Atomically reserve worst-case bounded call cost before dispatch and settle actual usage afterward. If the provider price is unknown, do not launch a paid call under an unenforceable budget. Infrastructure spending and model usage are separate controls; see operations.

## Required abuse checks

Try cross-workspace IDs, malicious retrieved instructions, fake approval IDs, approval replay, expired approval, changed payload, revoked identity mid-run, secret-like content in logs, concurrent overspending and unlisted tools. Assert no unauthorized side effect and no leaked content, with a sanitized rejection event. Passing a finite test set is evidence for that set, not a universal security guarantee.
