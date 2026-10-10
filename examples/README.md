# Synthetic examples and contract fixtures

All records in this directory are invented demonstration data. They contain no real business, guest identity, contact information, password, lockbox code or access credential. They are not app implementation or measured outputs.

| File | Use |
|---|---|
| [business_knowledge.md](business_knowledge.md) | Canonical synthetic source, `src_demo_policy`, version 1 |
| [questions.jsonl](questions.jsonl) | Twelve unique message IDs plus one repeated ID for dedup testing |
| [eval_cases.jsonl](eval_cases.jsonl) | Six expected-behavior starter cases, not a full benchmark |
| [agent_config.json](agent_config.json) | One illustrative validated-shape configuration, not runtime-validated |
| [team_plan.json](team_plan.json) | Three-agent analysis example with typed dependencies |
| [approval.json](approval.json) | Illustrative pending approval for expanded source scope; hash computed for sample payload only |
| [tool_call.json](tool_call.json) | Illustrative tool result shape; not an executed call |
| [policy_defaults.json](policy_defaults.json) | Proposed resource bounds and paid-disabled default |

Contract labels such as `schema_version: 1` refer to this internal design. They do not claim external API compatibility. The future implementation must build executable schemas and validate these fixtures.

Question dataset expected count: 13 input rows, 12 unique record IDs, one duplicate removed under `by_record_id`. Primary categories over unique records: parking 4, check_in 3, wifi 2, water_heater 1, other 2. Assign the adversarial record to other and do not execute its instruction.

The small fixture adapter may use deterministic lookup to exercise W1. Label this clearly in CLI/UI and evidence. Do not report its results as LLM accuracy. The final held-out evaluation set is created later and must not reuse these exact fixtures as unseen data.
