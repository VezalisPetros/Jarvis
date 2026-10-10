# Decision register

This is the authority for decision status, not evidence of implementation. Latest user instructions override older suggestions. Review with [sources](SOURCES.md) and [ADRs](adr/README.md).

## AGREED

| ID | Decision | Basis |
|---|---|---|
| A01 | Jarvis as AI Business Executive | Visible Jarvis conversation and packaging request |
| A02 | Dynamic Agent Team Generation, configs and workflows from a safe catalog | Explicit dynamic-team selection and packaging request |
| A03 | Backend enforcement of permissions, isolation, budgets and approvals | Packaging request |
| A04 | Business knowledge, memory, orchestration, evaluation and observability | Packaging request |
| A05 | Voice-first plus Executive Dashboard final experience | Explicit user selection |
| A06 | Deep learning and a reliable cloud-deployed portfolio are the current goals; profit is not a design criterion | Latest packaging request |
| A07 | Text end-to-end first, then dynamic agents, teams, approvals, dashboard and voice; tests/tracing/cloud incrementally | Latest packaging request |
| A08 | A single agent is valid when a team is unnecessary | Latest packaging request |
| A09 | Documentation-only handoff ZIP, Markdown, stable tasks and repository memory | Latest packaging request |
| A10 | Greek explanations, English code and technical docs; small learning steps | Latest packaging request |
| A11 | First local step requires no paid service; no fake implementation, measurements or personal data | Latest packaging request |
| A12 | Use only what was discussed here; do not change vision or add features | Latest user clarification |

## DEFAULT proposals, not prior agreements

| ID | Default | Why / decision point |
|---|---|---|
| D01 | Synthetic guest-question scenario for W1/W2 | Uses the already discussed check-in/parking/Wi-Fi examples without real data; review at J002 |
| D02 | Python/FastAPI, React/TypeScript, PostgreSQL/pgvector, Docker | Builds on discussed stack and background; lock compatible versions during implementation |
| D03 | SQLite/local files and fixture adapter for first local slice | Makes the requested zero-paid-service start executable; replace deliberately at J009 |
| D04 | Explicit loop and database-backed bounded orchestration first | Makes agent mechanics teachable; framework choice remains revisitable |
| D05 | Azure Container Apps, Blob Storage, managed PostgreSQL, Key Vault, Monitor and GitHub Actions | Prior assistant proposal; not explicit user cloud-provider choice; decide before J020 |
| D06 | One owner per workspace, sequential execution first | Reduces initial complexity while preserving later two-tenant isolation tests |
| D07 | Numeric limits in policy_defaults.json | Testable bounds, not capacity promises or approved spend |
| D08 | Speech-to-text/shared text engine/text-to-speech adapter before real-time optimization | Staged voice learning; provider choice at J028 |
| D09 | Fields, states, internal endpoints and errors in specifications | Necessary engineering contracts; they were not individually agreed in conversation |
| D10 | Proposed evaluation sizes and thresholds | Starting rubric, not achieved performance; finalize before held-out evaluation |

## OPEN

See [OPEN_QUESTIONS](OPEN_QUESTIONS.md): model/provider, cloud account/region and budget, authentication implementation, voice provider, IaC tool, backup policy, and later optional web search. None blocks J001.

## SUPERSEDED historical proposals

| ID | Earlier proposal | Replacement / reason |
|---|---|---|
| S01 | A real owner, commercial validation and willingness to pay as early gates | A06: learning and portfolio now take priority; synthetic demo is sufficient |
| S02 | Voice spike before the first text workflow | A07: complete text workflow first; voice remains a final requirement |
| S03 | €1 / five-minute run aspiration treated as a fixed gate | Remains historical aspiration only; real budgets and measurements unresolved |
| S04 | Three agents as a mandatory team | No such user agreement; A08 explicitly permits one. Three is only a default maximum |

Do not rewrite these historical rows to imply the previous assistant's suggestions were user decisions. On change, append an ADR and mark the replaced decision superseded. Current specs must reflect the new decision; history remains.
