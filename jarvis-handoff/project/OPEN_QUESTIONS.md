# Open questions and decision deadlines

No question below blocks J001 or the first deterministic local text workflow. Record answers when actually supplied; do not ask all questions upfront.

| ID | Question | Temporary default | Must resolve by | Actual blocking condition |
|---|---|---|---|---|
| Q01 | Which real LLM/provider and model? | Fixture adapter; provider interface only | J033, mandatory before J030 live comparison | Live calls require selected compatible adapter, credentials and authorized budget; fake mode may continue |
| Q02 | Azure or another cloud; account and region? | Azure architecture proposal | J020 | Cloud creation blocked until account, region and provider selection exist |
| Q03 | Actual model/voice and infrastructure spending ceilings? | Paid calls disabled | First paid invocation or J020, whichever comes first | Null limits are not authorization; show current cost drivers before approval |
| Q04 | Which authentication provider/session design? | Loopback local dev identity only | J009 design; required before J020 exposure | No public or remote deployment with dev identity |
| Q05 | Speech provider and initial transport? | STT → conversation engine → TTS adapter | J028 | Live voice needs provider support/credentials; text work continues |
| Q06 | Bicep or Terraform? | Defer selection and compare at task start | J025 | Does not block earlier manual cloud learning |
| Q07 | Backup retention and deletion/restore policy? | Synthetic-only data, no real personal data | J026 | Restore rehearsal needs a concrete policy; real data needs prior review |
| Q08 | Is an orchestration framework justified? | Explicit loop + DB worker | Revisit if J012/J014 complexity proves excessive | Not a blocker; preserve external contracts and record rationale if changed |
| Q09 | Enable optional web research? | Disabled | J032 only | It is not a core release requirement; credentials/egress checks if implemented |

A real customer, pricing, revenue target and commercial sector are not current blockers. API keys, account IDs and secret values must never be pasted into these files. When a question closes, retain its answer and decision reference with a resolved status instead of erasing history.
