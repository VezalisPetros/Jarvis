# Cloud, operations and cost controls

Agreed: a credible cloud-deployed portfolio. Azure and the services below were proposed in the discussion and remain defaults. This package grants no subscription access or spending authorization. First local steps require no paid services.

## Staged deployment

1. J019 creates a tested container and local persistent configuration.
2. J020 deploys the existing text workflow as a private authenticated cloud smoke test, after credentials, region and spending limits are resolved. Voice/dashboard are not prerequisites. Use synthetic data.
3. J024 adds automated releases; J025 codifies the chosen infrastructure; J026 adds diagnostic and recovery evidence. Final integrated cloud verification happens at J031.

Default target: API and worker on Azure Container Apps, source/report objects in Blob Storage, state in managed PostgreSQL, secrets in Key Vault, monitoring in Azure Monitor. The worker topology must be checked at the cloud task: a polling worker may require an active replica; an event/scheduled job is a different deployment choice. Never assume scale-to-zero works for a polling workload without an activation mechanism. Choose the simplest verified option and record cost implications.

Use local file/SQLite adapters only for early development. They are not a durable cloud deployment plan. Before deployment complete the PostgreSQL, tenant access and restart tasks. Cloud identity controls supplement application permissions; they do not replace them.

## Credentials and configuration

Required later: selected provider credentials if using a hosted model, an Azure subscription/account, resource permissions, region, and explicit model/cloud budget. Secrets are supplied outside chat/repository. Use managed identities and least-privilege resource grants where supported. Separate development and deployed configuration. Record names of required variables, never their values, in the future configuration guide.

Before a real cloud action, show the concrete resources and expected billing drivers using current provider pricing, then obtain authorization if not already given. Subscription/region availability and API compatibility must be verified at that point. No rates, free-tier entitlement or model/version availability are assumed.

## Two separate budgets

**Application budget:** token/call/step/time caps and a monetary ledger for model/tool calls. Reserve a conservative upper bound atomically before a call, settle afterward and reconcile uncertain outcomes. Parallel work cannot bypass the aggregate ceiling. Stop dispatch when a cap is reached and retain results. All planner, repair, reviewer, embedding and voice usage is counted.

**Infrastructure budget:** compute, database, object storage, registry, logs, backups and network. Provider budget alerts do not by themselves guarantee a hard spending stop. Use constrained resource sizes/replicas, retention limits, disabled idle resources where applicable and an explicit teardown checklist. Log ingestion and an always-running database/worker can cost money while no user is active.

[Policy defaults](../examples/policy_defaults.json) disable paid calls until real limits exist. Null financial limits mean unresolved and disallowed for paid execution, never unlimited. The €1 target in an earlier proposal was not authorization.

## CI/CD

A future pull request runs deterministic tests, schema checks and relevant integration tests without production credentials. Pin dependencies/actions at implementation time. The release builds an image once, records its digest, deploys a new revision, runs authenticated smoke checks and retains rollback instructions. Prefer short-lived federation for cloud CI access when configured; do not place long-lived credentials in source control.

Migrations require a compatibility plan: deploying the old image is not a safe rollback if the database schema is incompatible. Prefer additive schema changes, tested migrations and a recovery path before destructive changes. Do not automatically apply destructive migrations.

## Tracing and logs

Trace hierarchy: run → planning attempt → step/agent attempt → model/tool call. Common metadata: workspace pseudonymous ID, run ID, plan/config/prompt versions, tool version, duration, outcome, usage and sanitized error code. Do not log raw prompts, retrieved personal content, secrets or raw audio by default. Record enough event metadata to diagnose a failure without exposing content.

Start with structured local logs at J004, add traces at J023, then export to cloud monitoring. A trace error must not disable policy enforcement. Monitor queue/run age, step failure rate, retries, budget rejections and model usage. No service-level objective is claimed until measured.

## Recovery and operational runbook

- Worker restart: reconcile leases, persisted steps and uncertain tool outcomes; resume only eligible work.
- Database outage: reject/queue new work safely without confirming nonexistent persistence; show unavailable state.
- Object write failure: leave artifact incomplete and clean staged bytes using recorded ownership.
- Bad release: stop new runs, preserve checkpoints, route to a compatible previous revision and verify a smoke run.
- Restore rehearsal: restore database and object metadata into an isolated environment, reapply deletion tombstones, verify references and tenant boundaries, then record actual recovery time and data loss window.
- Cost incident: disable paid dispatch, stop schedules, inspect reservations and provider bills; reduce/stop infrastructure separately. Keep a resource inventory so teardown can be verified.

Restore times, availability, cost savings and load capacity are all unmeasured. The final portfolio should include a real deployment diagram, sanitized release evidence, one failure investigation and a measured cost example, only after they exist.
