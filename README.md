# Jarvis implementation and learning handoff

Jarvis is an AI Business Executive with dynamic agent team generation, a voice-first experience and an Executive Dashboard. The target is deep AI engineering learning and a reliable cloud-deployed portfolio project. Profit is not a design criterion at this stage.

**Release:** documentation handoff 1.0, 2026-10-10. **Implementation:** none. No app, credentials, dependencies, deployments or measured benchmark results are included. JSON files are specifications and synthetic examples, not a running application.

Start with [START_HERE_EL.md](START_HERE_EL.md), then [AGENTS.md](AGENTS.md) and [project/STATE.md](project/STATE.md). The first future implementation task is **J001**. Do not begin by building the entire application.

## Authority and provenance

This package organizes only the visible Jarvis discussion and the user's packaging instructions. The latest explicit instructions take precedence over earlier suggestions. It does not reconstruct omitted conversation. See [source mapping](project/SOURCES.md) and [decision register](project/DECISIONS.md).

- **AGREED:** explicit user requirements and choices.
- **DEFAULT:** a proposed, reversible implementation detail needed to make the agreed work actionable. It is not a prior user agreement.
- **OPEN:** unresolved decision, with its deadline and whether it blocks the current task.
- **SUPERSEDED:** earlier direction replaced by a later explicit instruction; retained for history.

Normative words MUST and MUST NOT express the proposed engineering contract for the agreed product. They do not imply that every field or algorithm was discussed previously. Do not turn defaults into historical agreements.

## File map

| File | Purpose |
|---|---|
| [START_HERE_EL.md](START_HERE_EL.md) | Transfer instructions and copyable Greek prompts |
| [AGENTS.md](AGENTS.md) | Codex startup, teaching, verification and checkpoint protocol |
| [Product and PRD](docs/01_PRODUCT_PRD.md) | Vision, user, workflows, scope and requirements |
| [Architecture](docs/02_SYSTEM_ARCHITECTURE.md) | Components, boundaries, stack and persistence |
| [Dynamic agents](docs/03_DYNAMIC_AGENTS.md) | Configurations, plans, validation and lifecycle |
| [Tools](docs/04_TOOL_REGISTRY.md) | Contracts, permissions, errors and side effects |
| [Security](docs/05_AUTONOMY_SECURITY.md) | Backend enforcement and approval semantics |
| [Knowledge](docs/06_KNOWLEDGE_MEMORY.md) | Facts, hypotheses, sources and corrections |
| [Orchestration](docs/07_ORCHESTRATION.md) | Dependencies, retries, recovery and cancellation |
| [Voice and dashboard](docs/08_VOICE_DASHBOARD.md) | Journeys, screens and interaction states |
| [Evaluation](docs/09_EVALUATION_TESTING.md) | Tests, metrics and three-way comparison |
| [Cloud and operations](docs/10_CLOUD_OPERATIONS.md) | Deployment, CI/CD, tracing, cost and recovery |
| [Roadmap](docs/11_DEVELOPMENT_ROADMAP.md) | Milestones and learning progression |
| [Contracts](docs/12_DATA_API_CONTRACTS.md) | Shared envelopes, conceptual records and API semantics |
| [Teaching guide](docs/13_LEARNING_WORKFLOW.md) | One-small-step teaching and understanding evidence |
| [STATE](project/STATE.md) | Current truth, active task, blockers and exact next action |
| [TASKS](project/TASKS.json) | Stable tasks, dependencies, acceptance and verification |
| [DECISIONS](project/DECISIONS.md) | Agreed, default, open and superseded choices |
| [ADR index](project/adr/README.md) | Significant decisions and their status |
| [OPEN_QUESTIONS](project/OPEN_QUESTIONS.md) | Deferred decisions and gates |
| [SESSION_LOG](project/SESSION_LOG.md) | Append-only checkpoint history |
| [LEARNING](project/LEARNING.md) | Reported background and demonstrated learning |
| [CHECKPOINT_TEMPLATE](project/CHECKPOINT_TEMPLATE.md) | Reusable session record |
| [Evidence index](project/evidence/README.md) | Rules for genuine execution evidence |
| [Package validation](project/evidence/PACKAGE_VALIDATION.md) | Documentation checks actually performed |
| [Examples](examples/README.md) | Synthetic fixtures and illustrative contract objects |

## Continuity model

Memory comes from files in this same repository and Git history. A new chat must open the same checkout, or a copy brought to the same revision. There is no invisible permanent memory or automatic synchronization between unrelated chats. Git commits also do not synchronize until changes are explicitly transferred or pushed and fetched.

Keep implementation, specifications, task status and evidence in the same reviewed change. Maintain one active implementation task. The packaged files are the starting state; later repository evidence can supersede them through the reconciliation protocol.
