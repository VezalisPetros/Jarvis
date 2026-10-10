# ADR 0002 Dynamic configurations and enforcement

Date: 2026-10-10

Status: Accepted principle; proposed contract mechanics

## Context

The user explicitly selected dynamic team generation rather than a fixed catalog of agent roles.

## Decision

Generate roles, configs and dependency plans dynamically from approved tools. Backend validation and runtime checks control execution. Choose one agent when sufficient.

## Alternatives

Fixed role catalog was offered but not selected. Arbitrary generated code is outside scope.

## Consequences

A bounded runtime can execute varied teams while denying unknown capabilities. Detailed schema and maximum sizes remain proposed defaults.

Affected files and tasks: docs/03_DYNAMIC_AGENTS.md, docs/04_TOOL_REGISTRY.md, docs/05_AUTONOMY_SECURITY.md; J006–J016.

Supersedes: None.

Superseded by: none.
