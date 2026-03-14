---
name: initiative-state-manager
description: 'Manage initiative memory, resume work safely, normalize shared artifacts, and reopen upstream phases when downstream findings invalidate stale assumptions. Use when reading or updating PHASE1_STATE.md, PHASE2_STATE.md, TEAM_MEMORY.md, DECISIONS.md, AGENT_HANDOFFS.md, or initiative artifact status.'
argument-hint: 'initiative folder, phase, or state management task'
user-invocable: true
---

# Initiative State Manager

Use this skill whenever an agent must resume an initiative, create or normalize shared memory files, update workflow state, or decide whether downstream work should reopen an upstream phase.

## When To Use

- Resuming work in `ai_docs/ideas/<initiative-slug>/`
- Initializing or refreshing `PHASE1_STATE.md` or `PHASE2_STATE.md`
- Updating `TEAM_MEMORY.md`, `DECISIONS.md`, or `AGENT_HANDOFFS.md`
- Determining whether an artifact is current, stale, blocked, or needs revision
- Choosing whether to continue forward or reopen an upstream phase

## Procedure

1. Resolve the initiative folder and inspect the shared state files before acting.
2. Classify the current phase status as `In Progress`, `Complete`, `Blocked`, or `Needs Revision`.
3. Identify which artifacts are authoritative for the current phase.
4. Normalize missing or malformed state files against the relevant templates without replacing strong existing content.
5. Update `TEAM_MEMORY.md` with new facts, assumptions, risks, or unresolved questions.
6. Update `DECISIONS.md` only for durable decisions with rationale.
7. Update `AGENT_HANDOFFS.md` with the current owner, next owner, required inputs, and unresolved blockers.
8. Update the phase state file with stage, owner, blockers, and next required actions.
9. If a downstream artifact contradicts an upstream assumption, mark the impacted artifact `Needs Revision` and reopen the upstream phase instead of proceeding with stale work.

## Reopen Rules

- Reopen discovery if the idea handoff lacks critical product context.
- Reopen PRD if UX, UI, test, security, or release-safety review finds a material contradiction.
- Reopen implementation if acceptance, E2E, or operate findings reveal missing or incorrect behavior.
- Reopen shipping if release verification, GitHub controls, or operating signals invalidate release readiness.

## References

- [State checklist](./references/state-checklist.md)
