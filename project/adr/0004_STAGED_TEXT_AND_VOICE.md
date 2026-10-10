# ADR 0004 Text first and voice destination

Date: 2026-10-10

Status: Accepted sequence; proposed adapter mechanics

## Context

The final user experience is voice-first; the latest request explicitly starts implementation with a text workflow.

## Decision

Complete W1 locally before dynamic loops/teams, approvals, dashboard and voice. Add tests/tracing early and a cloud smoke deployment before the final voice integration.

## Alternatives

An earlier voice spike before text is superseded. A staged STT/TTS adapter is a default, not a selected provider.

## Consequences

Text is an implementation stage and fallback; it does not remove voice from the final MVP.

Affected files and tasks: docs/08_VOICE_DASHBOARD.md, docs/11_DEVELOPMENT_ROADMAP.md; J003, J020–J029.

Supersedes: Earlier PRD sequencing, decision S02.

Superseded by: none.
