# Tool registry and contracts

Tools expose bounded application functions. A model chooses from schemas but never determines its actual authority. Registry entries are versioned and changed by developers, not generated agents. Contracts below are proposed internal interfaces, not implemented endpoints.

## Common invocation

Trusted runtime context: workspace_id, actor_id, run_id, step_id, policy_version, plan_version, trace_id, deadline and idempotency_key. This context is injected by the backend, not editable model arguments. The model provides only the tool-specific `arguments` object.

Return envelope: `{call_id, status, data, error, source_refs, usage}`. `status` is `succeeded`, `failed` or `needs_information`. Success has non-null data and null error. Failure has null data and `{code, message, retryable, retry_after_ms}`. Messages are sanitized. Usage records measured duration and provider usage when known; unknown monetary cost is null, never falsely zero. A permission denial is not retryable.

Every call logs metadata before dispatch and after result, checks payload size, scopes, budget reservation and deadline, and sanitizes returned text. See [data contracts](12_DATA_API_CONTRACTS.md) and [security](05_AUTONOMY_SECURITY.md).

## Catalog

| Tool ID | Arguments → data | Scope / effects | Failure behavior |
|---|---|---|---|
| `knowledge.search` | query, source_refs, limit → matches with text, source/version/location | `knowledge:read`; read only | Missing/deleted source fails; zero hits returns an empty list, not invented evidence |
| `dataset.aggregate` | dataset_ref, grouping_artifact_ref, dedup_mode → counts, denominator, excluded_count | `dataset:read`; read only | Invalid row IDs or duplicate group assignments fail validation |
| `draft.compose` | question, evidence_refs, language → grounded draft or gap | `draft:write`; creates internal draft | Absent or conflicting evidence returns needs_information; model usage is metered |
| `report.create` | validated_sections, artifact_refs → report_ref | `artifact:write`; internal write | Reject dangling/stale references; repeated idempotency key returns original report |
| `task.create` | title, goal_ref, artifact_refs → internal_task_ref | `task:write`; internal write | Invalid goal fails; does not invoke external task systems |
| `artifact.read` | artifact_ref → authorized content and status | `artifact:read`; read only | Cross-workspace/missing returns a non-disclosing not-found response |
| `memory.propose` | proposed claims, source_refs → pending_claim_refs | `memory:propose`; writes hypotheses only | Cannot confirm owner facts or change policy; invalid evidence rejected |
| `web.research` | public_query, result_limit → source URLs and excerpts | `web:read`; external request, disabled initially | Requires implemented egress checks and authorization; timeout or denied domain recorded |

`draft.compose` may call the model adapter inside its implementation. Count that usage once under a child trace, not again as an unmetered hidden call. Artifacts created by a tool are scoped by trusted context. Their IDs do not grant access.

## Detailed restrictions

**Knowledge search:** source refs are exact versions. Enforce a server-side result limit (default 5), chunk-size limit and tenant predicate before results leave the database. Return citations as source ID/version plus page, paragraph or row locator. Do not return storage credentials or signed upload URLs. A match score is not factual confidence.

**Dataset aggregate:** the model may propose category assignments, but the tool counts deterministically. Default dedup_mode is `by_record_id`; identical texts with different IDs count as distinct messages. One primary category per included record, including `other`; category counts must sum to denominator. Multi-label counts are a separate contract, not silently mixed. Percentages state their denominator and rounding method. No arbitrary SQL argument.

**Draft compose:** evidence refs must resolve to allowed, active source versions. Output includes draft text, claim-level citations, missing fields and status. Do not reveal access codes or make unsupported operational promises. A language instruction changes wording, not facts. Quotes from malicious sources remain data.

**Report/task creation:** these are application writes, not filesystem access. Names cannot choose arbitrary paths. Use a uniqueness constraint on workspace plus idempotency key. A repeated key with a different input hash returns `IDEMPOTENCY_CONFLICT`.

**Web research:** optional later capability. No private business facts, credentials or raw customer messages in search queries. Fetching must reject local/private network addresses, metadata endpoints and unsafe redirects, with response-size/time limits. Search results cannot activate tools. Implement this only at J032, after core goals, if still wanted.

## Limits and errors

Initial defaults live in [policy_defaults.json](../examples/policy_defaults.json). Errors: `VALIDATION_ERROR`, `UNKNOWN_TOOL`, `SCOPE_DENIED`, `APPROVAL_REQUIRED`, `STALE_SOURCE`, `BUDGET_EXHAUSTED`, `TIMEOUT`, `RATE_LIMITED`, `UPSTREAM_UNAVAILABLE`, `IDEMPOTENCY_CONFLICT`, `CANCELLED`, `INTERNAL_ERROR`.

Only timeout/rate-limit/upstream transient failures are eligible for bounded retry, and only when the tool is read-only or idempotent. A write timeout with unknown outcome triggers reconciliation by idempotency key before retry. Failed calls and retries consume budget when the provider bills them.

## Registry record

Each entry records tool ID/version, input/output contract IDs, required scopes, side-effect class, approval rule, idempotency support, timeout, egress policy and enabled environments. [tool_call.json](../examples/tool_call.json) is an illustrative successful result. The registry MUST be independently tested with forbidden tools and scopes; giving the LLM a list is not enforcement.
