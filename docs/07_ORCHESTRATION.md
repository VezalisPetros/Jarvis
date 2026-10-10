# Orchestration and execution state

This is the canonical proposed state machine. Client views and task framework adapters must map to it. See [data contracts](12_DATA_API_CONTRACTS.md) for version and idempotency fields.

## Run transitions

| From | Trigger | To |
|---|---|---|
| draft | Plan submitted | validating |
| validating | Valid and authorized | queued |
| validating | Valid but no matching grant | awaiting_approval |
| validating | Missing required information | awaiting_input |
| validating | Unrecoverable invalid plan | failed |
| awaiting_approval | Valid approval granted | queued |
| awaiting_approval | Denied or expired | paused, with reason and no dispatch |
| awaiting_input | New input changes plan | validating, with a new plan version |
| queued | Worker acquires lease | running |
| running | Need input or material approval | awaiting_input or awaiting_approval |
| running | Owner pause or resource limit | pausing, then paused at safe boundary |
| paused | Resume request and fresh validation | validating |
| running | All required steps and output checks pass | completed |
| running | Required output missing but usable artifacts remain | partially_completed |
| running | No usable required output after terminal failure | failed |
| Any nonterminal | Authorized cancellation | cancelling, then cancelled after in-flight reconciliation |

Terminal states: completed, partially_completed, failed, cancelled. Re-running terminal work creates a linked new run, not a rewritten history. Invalid transitions return a conflict. Run version protects against two clients applying incompatible actions.

## Step transitions and dependencies

Steps move pending → ready → running → succeeded or failed; pending/ready can become skipped or cancelled; running can become waiting_input, waiting_approval or retry_wait. Leases and reservations are released or reconciled when leaving running. A succeeded required predecessor is necessary for a dependent step to become ready. Failed required predecessors skip dependent work with `dependency_failed`.

Optional-step failure does not automatically fail the run if all required deliverables and quality checks succeed; record warnings. If partial output is useful but required coverage is absent, mark partially_completed and show missing sections. A reviewer cannot mark the run complete without the deterministic artifact checks.

## Handoffs

Pass typed artifact references and a short summary containing completed work, unresolved questions and source refs. Do not copy arbitrary full agent transcripts into the next agent. Resolve artifacts through the tool gateway under the next step's permissions. Input contracts and source versions must be checked at consumption time.

## Persistence and leases

Persist run, step, attempt, call and artifact identities. At a step boundary, commit artifact metadata, step result and checkpoint atomically where possible; object bytes can be staged first, finalized by hash, and orphaned bytes cleaned with a documented process. Checkpoint includes plan/config versions, completed step IDs, artifact refs, pending waits and budget ledger state. Never rely on Python memory after a process dies.

A worker claims a ready step with a lease, attempt ID and fencing generation. Heartbeats renew the lease. After expiry another worker may claim it. Results from stale generations cannot overwrite newer state. Use transactional conditions/locks to avoid double ownership. Start with sequential execution; add concurrency only after failure tests.

## Retries and unknown outcomes

Default two retries after the first call for transient errors, with bounded backoff and optional retry_after. All attempts count toward call and cost ceilings. Validation, permission and stale-source errors are not retried blindly. Cancellation interrupts backoff.

Idempotency key is stable for the logical side effect across attempts. If a write times out, reconcile by that key before another write. A crash after provider response but before recording may incur another bill on replay; budget accounting must track uncertain usage. Do not promise exactly-once execution across external systems. Provide at-least-once attempts with deduplicated supported writes.

## Pause, resume and cancel

Pausing stops new calls; an existing call may finish. Resume requires current identity, sources, plan and policy validation. Cancellation records the request before signalling workers and blocks dispatch atomically with the budget/lease check. Cancelled model requests may still be billed. Preserve outputs already created and label them with final state; cancellation is not rollback.

If a worker cannot be reached, keep cancelling with a visible reconciliation reason until its lease/outcome is resolved. A timeout cannot be disguised as successful cancellation. A global workspace pause prevents new runs and new tool dispatches in existing runs.

## Scheduled briefing

Default weekly schedule is opt-in, with explicit IANA timezone, local weekday/time and budget. No schedule is activated by creating this package. Deduplicate by workspace, schedule version and period key. Propose one catch-up briefing for a missed period instead of replaying unlimited history. Briefing includes actual completed outputs, unresolved work and missing facts; it may propose a new agent but not activate an ungranted workflow. Define and test daylight-saving behavior before enabling schedules.
