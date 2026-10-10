# Learning workflow

Agreed: explanations in Greek, code and technical documentation in English. The builder has programming, RAG and LLM evaluation experience. New emphasis: agent control flow, orchestration, voice and cloud. Source learning roadmap remains conceptual background; this project does not claim every curriculum module is completed.

## One task cycle

1. Explain what we are building and the observable result.
2. Introduce the prerequisite concept with a small example from Jarvis.
3. Compare realistic alternatives and explain the selected default and tradeoff.
4. Implement one small complete slice, within the selected task's scope.
5. Verify with a command, test or reproducible manual procedure; show meaningful outcome and failure behavior.
6. Recap and ask a short understanding question or exercise where useful.
7. Update STATE, TASKS, SESSION_LOG and LEARNING; leave one precise next action.

A task can span several chats. Work that is complete technically may still have unassessed learning. Never gate every reversible edit on permission, but do stop at a sensible learning boundary. If the owner cannot explain a concept, record the gap and use a targeted example before expanding it.

## Progress states

Learning states: not_assessed, introduced, practiced, demonstrated, needs_review. Record evidence and date separately for each. A chat explanation introduces a concept; an owner explanation or exercise can demonstrate it. Generated code alone demonstrates neither practice by the owner nor mastery.

Implementation states live only in TASKS: todo, in_progress, blocked, done. Do not conflate them with learning states.

## Questions tied to milestones

| Topic | Example check |
|---|---|
| Workflow versus agent | Which part decides the next action in each version of W1? |
| Tool contracts | Why is a tool name in a prompt insufficient authorization? |
| Dynamic teams | When is one agent better, and how would you measure team overhead? |
| State and retries | What happens if a report is saved just before the worker crashes? |
| Approvals | Why must an approval refer to an exact plan version? |
| Memory | Why is an inferred business preference not a confirmed fact? |
| Tenant isolation | Where could a cache or background worker leak another workspace's data? |
| Cloud | What continues to cost money when no one uses the app? |
| Voice | Why should interrupting speech not automatically cancel a task? |
| Evaluation | What must remain equal when comparing three execution strategies? |

No requirement to relearn basic RAG from scratch. Revisit fundamentals when a concrete gap affects the work. Fine-tuning, transformer internals and advanced infrastructure remain broader curriculum topics, not extra Jarvis MVP features.
