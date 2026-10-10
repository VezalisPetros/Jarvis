# Dynamic agent specification

Agreed: agents and workflows are generated dynamically from a safe catalog. One agent is valid when sufficient. Default below: typed configurations and bounded dependency graphs. [Security](05_AUTONOMY_SECURITY.md) is authoritative for permissions.

## Configuration contract

A configuration is data interpreted by a trusted runtime, not generated executable code. Strict validation rejects unknown fields rather than silently ignoring attempted capability additions.

| Field | Constraint |
|---|---|
| schema_version | Internal contract version, initially `1`; not a provider API version |
| agent_id / config_version | Server-assigned identity and immutable increasing version |
| role / objective | Bounded plain text; no authority to alter policy |
| knowledge_refs | Explicit source ID/version pairs from this workspace |
| tool_ids | Subset of the versioned registry and current actor authorization |
| output_contract | Existing catalog contract such as `grounded_answer.v1` or `analysis_report.v1` |
| limits | Steps, tool calls, timeout and output limits no higher than effective policy |
| escalation | Fixed enum: `needs_information`, `approval_required`, `fail` |
| provenance | Planning run, prompt version, model identifier if applicable and generation time |

See the synthetic [agent example](../examples/agent_config.json). Agent instructions are bounded subordinate task instructions. Arbitrary package imports, shell scripts and self-created tool definitions are rejected. Output schemas are selected from vetted contracts in the MVP; arbitrary output shapes can be considered later only through an ADR.

## Team plan contract

A plan has server-assigned plan ID, immutable plan version, objective, source snapshot, referenced agent configurations, ordered-independent step objects, deliverable contract, limits and validation result. Each step has a unique ID, agent reference, explicit dependencies, typed input bindings and a required flag.

Bindings reference initial input or previous step artifact IDs, not free-form transcript concatenation. A dependency can consume only a compatible output contract. The whole graph must be acyclic and reachable from initial inputs. Reject orphan steps, duplicate IDs, dangling references, cross-workspace data, invalid source versions and absent deliverable producers.

The architect proposes the smallest reasonable plan and gives a short operational rationale. It may select one agent for a straightforward grounded answer; repeated-question analysis may justify an analyst, response drafter and reviewer. These are examples of generated roles, not a fixed mandatory team. See [team example](../examples/team_plan.json).

## Validation sequence

1. Parse strict schema and enforce size limits before deeper processing.
2. Resolve server-side workspace, source snapshot, registry and policy versions.
3. Check agent counts, referenced contracts, allowed tools and limits.
4. Validate unique step IDs, dependency existence, acyclic graph and compatible bindings.
5. Ensure all required deliverables are reachable; define handling of optional-step failure.
6. Compute effective scopes and budget; never copy a requested privilege into a grant.
7. Persist the immutable validated plan with canonical payload hash and validation report.
8. Authorize or request approval through the backend. Recheck before every tool/model call.

Default: at most two plan-repair attempts after the initial proposal; all attempts count against limits. Return a structured list of error codes/paths to the architect, without secrets. Exhaustion produces `needs_information` or a failed planning outcome, not an unvalidated execution.

## Lifecycle and versioning

Configuration: proposed → validated → active → retired. Rejection retains a sanitized validation record. Editing a validated object creates a new version. Running work references the exact version it began with. A new plan version invalidates approvals for the prior payload. Do not hot-swap tools, model instructions or data scopes within an active plan.

Agent execution: pending → running → succeeded/failed/cancelled; waiting states follow the run/step lifecycle. Configurations can be reused only after revalidation against current policies, tools and sources. Persist retirement history without making retired configurations eligible for new runs.

## Delegation invariants

The executive can request a plan; a generated agent cannot directly spawn unbounded children. Any new delegation goes through the architect and validator as a new plan version. Effective permissions are the intersection of owner grants, workspace policy, approved plan scope and runtime capability. A child's permissions and budget cannot exceed the parent's. Tool results cannot grant more authority.

The plan size is bounded by [defaults](../examples/policy_defaults.json). Initial execution can be sequential even for independent steps. Parallel execution is allowed only after J014 proves lease, budget and cancellation behavior.

## Acceptance examples

- Unknown `send_email` tool: reject `UNKNOWN_TOOL`; no external side effect.
- Two steps depending on each other: reject `DEPENDENCY_CYCLE` before queueing.
- Retrieval source from another tenant: reject `SCOPE_DENIED`, without exposing source content.
- Valid single-agent response: accept without manufacturing extra agents.
- Plan requires more calls than policy allows: return `LIMIT_EXCEEDED`; repair or stop.
- Replanned objective while awaiting approval: retain old plan for history, mark approval stale and validate the new one.
