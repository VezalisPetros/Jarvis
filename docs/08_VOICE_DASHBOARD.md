# Voice and Executive Dashboard

Agreed destination: natural voice interaction plus full oversight. Agreed sequence: text workflow first, then dynamic agents/teams, approvals, dashboard and voice. Voice-first is the final user experience, not the first implementation task.

## Screens and visible behavior

| View | Required content and actions |
|---|---|
| Conversation | Shared voice/text history, uploads, source links, targeted questions and run references |
| Overview | Goals, active work, waiting approvals, latest briefing and known usage/budget |
| Runs | Actual step statuses, warnings, results, retry history, pause/resume/cancel |
| Team | Generated roles, purpose, assigned tools, sources and configuration version |
| Approvals | Exact action/scope/budget/expiry, approve or deny, stale/consumed/revoked status |
| Results | Reports, internal drafts, claim citations, knowledge gaps and usefulness feedback |
| Knowledge/settings | Facts versus hypotheses, corrections/deletion, sources, grants, cost limits and schedule |

On mobile, conversation is primary with accessible navigation to work and approvals. On desktop, conversation and oversight can coexist. Never manufacture a progress percentage. Render `awaiting_input`, `awaiting_approval`, `paused`, `partially_completed` and `failed` distinctly. Unknown cost is shown as unknown or pending settlement, not zero.

## Text journey before voice

Owner starts W1 → selects available sources → receives run ID → observes actual state → opens the draft and citation → corrects a missing fact if needed. Reconnecting or refreshing must read the same run. Submission includes a client request ID; a repeated network delivery cannot create a second run. Selecting another workspace cannot retain the first workspace's cached artifacts.

## Voice adapter journey

Initial voice default is a replaceable speech-to-text → shared conversation engine → text-to-speech adapter. This makes transcript and state boundaries inspectable. A real-time provider can be selected after measuring the simpler path. Provider, SDK, model and version remain open until J028; no claimed latency is assumed.

Voice states: idle, requesting_permission, listening, transcribing, thinking, speaking, interrupted, reconnecting, error. These describe the conversation channel, not the background run. Show the distinction between “Jarvis is speaking” and “analysis is running.”

When the owner speaks over Jarvis, stop audio output and discard queued stale audio for that turn. Do not automatically cancel background work. When the owner explicitly requests “stop the task,” call the same authorized cancel endpoint used by the dashboard. The UI acknowledges cancellation status, including calls already in flight.

If transcription is uncertain about a meaningful name, number or action, ask a targeted clarification. Allow transcript correction before consequential actions. A pending approval still needs a visible authenticated confirmation card. Microphone denial, provider error or disconnection offers immediate text fallback without losing the conversation ID.

## State and accessibility

Use stable message IDs, run IDs and monotonically increasing event sequence per run. Events are hints; reconnect fetches authoritative state and optionally resumes after the last event ID. Do not replay user actions when replaying events. Keyboard and screen-reader accessible controls must exist for voice functions; transcript is visible and usable without audio.

Raw audio is not stored by default. Test stop-speaking, disconnect/reconnect, denied microphone, changed transcript and conversation switch. Synthetic conversations only for the portfolio. Test target browsers at J029 and record actual versions then, rather than claiming compatibility now.

## Acceptance

A user can start by text, inspect the same task in dashboard, continue by voice and return to text without duplicated work. A stale approval card cannot execute changed work. A failed step exposes a useful sanitized error and preserved partial artifacts. Citations resolve to the authorized exact source version. The generated team view never claims agents are active when the run has finished.
