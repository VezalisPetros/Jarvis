# ADR 0005 Repository continuity and learning evidence

Date: 2026-10-10

Status: Accepted by explicit user instruction

## Context

New chats must reconstruct progress without relying on invisible memory.

## Decision

Maintain AGENTS, STATE, TASKS, DECISIONS, OPEN_QUESTIONS, SESSION_LOG and LEARNING in the same repository. Reconcile them against code and Git each session.

## Alternatives

Independent chat histories are insufficient and are not automatically synchronized.

## Consequences

Checkpoint updates are part of every meaningful task. Abrupt interruption requires later reconciliation; learning is never inferred from generated code.

Affected files and tasks: AGENTS.md and all project state files; every task.

Supersedes: None.

Superseded by: none.
