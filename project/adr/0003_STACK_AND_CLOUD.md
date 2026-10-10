# ADR 0003 Stack and cloud defaults

Date: 2026-10-10

Status: Proposed default, not a prior user agreement

## Context

The cloud discussion proposed Azure to provide practical deployment evidence. The handoff also requires a zero-paid-service local start.

## Decision

Use Python/FastAPI and React, local fixture/file/SQLite adapters first, PostgreSQL before concurrent tenant execution, and an Azure deployment path. Keep model and speech behind adapters.

## Alternatives

A different cloud or orchestration framework can be selected if justified; multi-cloud and Kubernetes add no agreed MVP requirement.

## Consequences

Provider/account/budget and exact versions remain unresolved. Migration and authentication are required before cloud exposure.

Affected files and tasks: docs/02_SYSTEM_ARCHITECTURE.md, docs/10_CLOUD_OPERATIONS.md; J002–J005, J009, J019–J026.

Supersedes: None.

Superseded by: none.
