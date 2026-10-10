# Evaluation and testing

No application tests or model benchmarks have run. Package checks are documentation checks only. The goals below are proposed targets, not reported results. [TASKS](../project/TASKS.json) owns implementation verification evidence.

## Progressive test layers

1. First local slice: fixture input/output, missing evidence, citation locator validity, deterministic counting and persisted artifacts. No network or credential dependency.
2. Tool and agent contracts: strict schemas, malformed model output, unlisted tools, step limits, plan cycles, invalid handoffs and abstention.
3. Persistence and security: restart recovery, duplicate requests, lease expiry, approval replay, revocation, deletion and cross-workspace access.
4. UI and voice: visible run states, disconnect/resume, microphone denial, interruption and text fallback.
5. Cloud: authenticated access, deployment/rollback, redacted traces, budget blocking, recovery rehearsal and resource teardown.

Tests must assert observable behavior, not merely mirror implementation. Inject failures at boundaries and verify no unauthorized side effect. Keep network/provider tests opt-in and separately labeled; deterministic unit tests must remain runnable without paid services.

## Dataset design

[examples/questions.jsonl](../examples/questions.jsonl) and [knowledge](../examples/business_knowledge.md) contain synthetic guest questions and explicit evidence. They are starter fixtures, not a statistically representative evaluation set. The included [gold cases](../examples/eval_cases.jsonl) specify expected behavior without storing fabricated model outputs.

Later construct and freeze a versioned development set and a separate held-out set. Include grounded questions, missing facts, conflicting source versions, corrected/deleted data, ambiguous intent, repeated records and prompt injection. Do not tune prompts on held-out failures and then still call the set held-out; version the split and disclose reuse.

Default final target: at least 50 held-out questions including at least 10 missing-evidence cases; 20 plan requests with both simple and multi-step tasks; at least 10 onboarding cases. Dataset size is a planned target, not an existing asset. Record dataset hash, labeling rubric, split, source version, prompt/config versions, model/provider identifier, parameters, run count and environment.

## Three-way comparison

| Variant | Control flow | What it tests |
|---|---|---|
| Fixed workflow | Predetermined retrieval/analysis/drafting/review steps | Reliability of known routing |
| Single agent | One agent chooses allowed actions in a bounded loop | Value of adaptive tool selection |
| Dynamic team | Architect generates one or several agents and a validated plan | Value and overhead of dynamic decomposition |

Use the same task set, evidence, model family where feasible, output rubric and maximum budget envelope. Include planning, repair, review and failed-call costs for the dynamic variant. Report differences in actual tools/model settings rather than implying perfect control. Repeat stochastic runs under a declared protocol, randomize evaluation order where useful, and report distributions plus failures. Do not presume that dynamic teams win; a simpler task should often select one agent.

## Metrics and definitions

- Task success: required artifact contract and evidence/coverage checks pass, not merely HTTP success.
- Grounded answer correctness: human rubric checks factual correctness, evidence support, relevance and proper abstention. A model judge is supplementary, calibrated against human review.
- Citation validity: cited source/version/locator exists and supports the associated claim.
- Valid plan rate: fraction passing deterministic validation within the repair limit; report initial and repaired rates separately.
- Abstention rate on unsupported cases: explicit gap/clarification without invented answer.
- Latency: total run duration including planning/retries; voice first-audio latency measured separately from end-of-speech.
- Cost: actual usage times dated applicable prices plus tool charges when known. Report unknown values and infrastructure separately.
- Human intervention: count clarification/approval events separately; expected approvals are not automatically failures.

## Proposed acceptance targets

| Area | Target before portfolio demonstration |
|---|---|
| Deterministic controls | All defined permission/isolation/budget/replay/cancellation tests pass; no observed unauthorized side effect |
| Plan generation | At least 19/20 valid plans within two repair attempts; show rejected cases |
| Main workflow | At least 18/20 complete required artifacts under the declared protocol |
| Grounded answers | At least 45/50 held-out answers pass the human rubric |
| Missing evidence | At least 9/10 unsupported questions appropriately abstain |
| Profile extraction | At least 90% correctly represented fields across 10 scenarios; distinguish inferred versus confirmed |
| Recovery | Required crash/duplicate/lease cases pass without duplicate internal writes |
| Voice | Measure p50/p95 and interruption latency; earlier aspiration of p95 under 3 seconds for simple no-tool turns is provisional |

The earlier five-minute/€1 run aspiration is not an approved spending limit or a pricing estimate. Do not make it a mandatory gate before provider and budget decisions. Record measured values first and set justified targets at J030. Runtime hard spending limits are chosen separately before paid calls.

## Reporting

For every benchmark retain sanitized input IDs, expected criteria, actual outputs, trace IDs, evaluator decisions and failures. Keep a per-variant summary with dataset count and denominator. Publish only measurements actually produced. If a gate fails, describe the failure and remediation; do not delete difficult cases to claim success.
