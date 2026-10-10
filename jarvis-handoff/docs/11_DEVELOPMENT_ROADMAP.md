# MVP development roadmap

The authoritative detailed backlog is [TASKS.json](../project/TASKS.json): every item has goal, deliverables, dependencies, related specs, acceptance criteria, verification plan, learning objective, credentials and cost conditions. IDs are stable. Current state is [STATE](../project/STATE.md).

## Milestones

| Milestone | Tasks | Observable exit condition |
|---|---|---|
| M0 Handoff and readiness | J000–J001 | Documentation verified; actual repository/environment reconciled |
| M1 Local text workflow | J002–J006 | Complete fixture-based W1, persistence, bounded tools, no paid dependency |
| M2 Dynamic generation | J007–J011, J033 | Bounded single agent, generated configurations/teams, PostgreSQL isolation and W2 report |
| M3 Durable control | J012–J016 | Restart/retry/approval/cancellation/revocation behavior verified |
| M4 Business memory | J017–J018 | Versioned facts/sources, corrections and deletion propagation |
| M5 Cloud foundation | J019–J020 | Containerized text workflow and first authenticated cloud smoke result |
| M6 Executive oversight | J021–J023, J027 | Dashboard, approval/result views, traces and opt-in briefing |
| M7 Cloud operations | J024–J026 | Tested releases, IaC and actual recovery rehearsal |
| M8 Voice | J028–J029 | Shared voice/text context, interruption and fallback demonstrated |
| M9 Portfolio evidence | J030–J031 | Honest three-way evaluation and integrated deployed demo |
| Later optional | J032 | Controlled web research, only if enabled later |

## Order and parallel eligibility

Take the earliest eligible task unless STATE records an unfinished one. Some tasks have independent branches: cloud credentials at J020 need not block local dashboard tasks J021/J022; Q01 live-model access need not block offline contract work; J033 is a separate live-integration task and can run as soon as its dependencies and credentials are ready. Record blocked live verification explicitly rather than silently claiming the model works.

J005 proves only the adapter contract with offline fixtures. J033 requires actual live-model evidence and remains blocked if provider access is unavailable. J030 and J031 cannot use fixture success as a substitute for real provider/deployment results. Stable task IDs do not imply numerical execution order; dependencies control eligibility.

Cloud verification occurs before final voice integration; voice-first remains the final product experience. A later reordering can be recorded if a genuine dependency emerges, but do not remove dynamic teams or voice to finish sooner.

## Definition of done for each task

Required acceptance criteria have evidence; relevant tests pass; limitations and not-run checks are explicit; specs match code; STATE/TASKS/SESSION_LOG updated; learning state assessed separately. A task blocked on credentials stays blocked or is split honestly. No fixed delivery dates or estimated implementation hours are promised.

## First implementation session

J001 only: inspect repository and local runtime, reconcile state, explain the architecture map in Greek, record prerequisites and exact next action. Do not install unrelated stacks or build the whole system. The next session begins J002: typed contracts, fixture adapter interface and a tiny runnable test setup. J003 then completes the first text workflow.

## Learning scope

Every task uses [teaching workflow](13_LEARNING_WORKFLOW.md). Revisit concepts when needed; do not force the user through a long generic course before seeing a complete result. Conceptual clarity, practical verification and explanatory ability are all tracked, with no automatic mastery marks.
