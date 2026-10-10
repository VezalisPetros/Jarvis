# Product vision and PRD

Status: specification, no implemented features. Authority: [decisions](../project/DECISIONS.md). The product goals are agreed; detailed acceptance contracts are proposed defaults.

## Vision and user

Jarvis is an AI Business Executive: it learns business context, asks targeted questions, identifies operational needs, proposes work, dynamically configures agents, executes permitted workflows and explains outcomes. The final experience is voice-first with an Executive Dashboard. The learning path starts in text.

The product user is a small-business owner who can describe goals but should not need to design agent teams. The builder's primary goal is deep AI engineering understanding and a credible cloud portfolio. A real business, revenue, pricing and willingness to pay are not prerequisites. Use the guest-question scenario already discussed as a synthetic demonstration, not a decision to build a hospitality-only product.

## Demonstration workflows

**W1 Grounded answer, first local slice.** Owner submits “Where can guests park?” with a selected knowledge set. Retrieve a source, produce an internal draft with precise source/version/location references, store the artifact and show its status. If the source does not say, return needs_information and a targeted question. The first deterministic adapter only demonstrates this data flow.

**W2 Repeated-question analysis, main end-to-end workflow.** Owner supplies synthetic customer questions and business instructions. Jarvis identifies categories, counts them deterministically, prepares grounded draft responses, highlights unanswered/conflicting issues and produces a report. The dynamic planner selects one agent or a bounded team. The owner can inspect evidence, costs and failures. No messages are sent to guests.

**W3 Executive briefing.** After explicit schedule activation, summarize goals, unfinished tasks, completed outputs and missing information. Propose follow-up work. New runs are permitted only under an existing matching policy or a new approval. A recurring briefing is not unlimited authority.

**W4 Business onboarding.** Ask one context-sensitive question at a time; distinguish owner-confirmed facts from model hypotheses; allow corrections. Reuse the same memory in text and voice. Questions about Wi-Fi credentials or a lockbox must surface a restriction or information gap; synthetic fixtures contain no actual codes.

## Requirements and traceability

| ID | Required final behavior | Primary specification | Tasks |
|---|---|---|---|
| FR01 | Text and voice share conversation and run state; interruption and text fallback | [UX](08_VOICE_DASHBOARD.md) | J003, J028, J029 |
| FR02 | Correctable business facts and goals with provenance | [Memory](06_KNOWLEDGE_MEMORY.md) | J017, J018 |
| FR03 | Grounded retrieval and source references; abstain on gaps | [Tools](04_TOOL_REGISTRY.md) | J003, J005, J017 |
| FR04 | Generate valid one-agent or multi-agent configurations and plans | [Agents](03_DYNAMIC_AGENTS.md) | J008, J010, J011 |
| FR05 | Validate tool scopes, data scope, schemas and acyclic plans | [Security](05_AUTONOMY_SECURITY.md) | J006, J007, J009 |
| FR06 | Persistent execution, checkpoints and recovery | [Execution](07_ORCHESTRATION.md) | J004, J012, J014, J015 |
| FR07 | Version-bound approvals, revocation and cancellation | [Security](05_AUTONOMY_SECURITY.md) | J013, J016 |
| FR08 | Dashboard with tasks, teams, evidence, failures and costs | [UX](08_VOICE_DASHBOARD.md) | J021, J022, J023 |
| FR09 | Opt-in periodic briefing with deduplication | [Execution](07_ORCHESTRATION.md) | J027 |
| FR10 | Feedback, corrections, deletion and stale-output handling | [Memory](06_KNOWLEDGE_MEMORY.md) | J018, J022 |
| FR11 | Cloud deployment, repeatable releases and recovery evidence | [Operations](10_CLOUD_OPERATIONS.md) | J019, J020, J024, J025, J026, J031 |
| FR12 | Compare fixed workflow, single agent and dynamic team fairly | [Evaluation](09_EVALUATION_TESTING.md) | J030 |

## Final MVP boundary

Includes bounded dynamic teams, business knowledge and memory, persistent orchestration, backend controls, internal reports/tasks/drafts, evaluation, tracing, a private cloud demo, voice interaction and Executive Dashboard. Optional web research remains later and disabled until its scope and egress checks are implemented.

Excludes arbitrary generated code execution, self-installed tools, unrestricted recursion, external email/CRM/calendar writes, transactions, contracts, autonomous price changes and direct guest support. These are later possibilities from the discussion, not hidden MVP commitments. Do not add unrelated business features, fine-tuning or Kubernetes to make the portfolio appear larger.

## Success and failure

Success means the builder can explain the architecture and demonstrate verified behavior, including failures, on synthetic cases. Outputs must include evidence or explicit uncertainty. A workflow completes only when its required artifacts and quality checks succeed. Partial outputs remain visibly partial. Final model benchmarks, latency and cloud cost are measured later; no success, learning mastery or production-readiness claim is made by this package.

A real-owner pilot may later strengthen evaluation but does not block any synthetic-data milestone. Product quality and engineering evidence replace the earlier commercial validation gate. See [roadmap](11_DEVELOPMENT_ROADMAP.md).
