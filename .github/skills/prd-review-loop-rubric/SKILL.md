---
name: prd-review-loop-rubric
description: 'Run a consistent PRD review loop with confidence scoring, blocker classification, revision focus, and pass criteria. Use when reviewing PRD.md, writing PRD_NOTES.md, updating PRD_REVIEW_STATE.md, or deciding whether planning can proceed.'
argument-hint: 'initiative folder or PRD review scope'
user-invocable: true
---

# PRD Review Loop Rubric

Use this skill when drafting, reviewing, scoring, or revising `PRD.md`.

## When To Use

- Reviewing `PRD.md`
- Writing `PRD_NOTES.md`
- Updating `PRD_REVIEW_STATE.md`
- Deciding whether UX, UI, testing, or delivery planning may proceed

## Review Dimensions

- problem clarity
- user and stakeholder clarity
- scope boundaries
- requirement specificity
- acceptance testability
- codebase and integration realism
- security and operational readiness implications
- unresolved risk posture

## Confidence Rubric

- 0.0-0.49: unclear or structurally incomplete
- 0.50-0.79: directionally solid but materially underspecified
- 0.80-0.90: nearly ready, but important details still weaken downstream confidence
- 0.91-1.00: ready for downstream planning with only non-blocking residual gaps

## Procedure

1. Read `PRD.md`, `TEAM_MEMORY.md`, `DECISIONS.md`, and `AGENT_HANDOFFS.md`.
2. Score the PRD on the review dimensions.
3. Separate findings into blocking gaps, non-blocking gaps, and editorial improvements.
4. Write concise revision instructions in `PRD_NOTES.md`.
5. Update `PRD_REVIEW_STATE.md` with cycle number, confidence score, pass/fail, blocking gaps, and next revision focus.
6. If confidence is not high enough, route the PRD back for revision instead of allowing downstream work.
7. If downstream specialists later find a material contradiction, reopen the loop with explicit revision focus rather than patching around the issue.

## Pass Criteria

- at least 2 review cycles completed
- confidence greater than 90%
- all blocking gaps resolved
- remaining gaps are explicitly non-blocking
- downstream artifacts are not being asked to invent missing product decisions

## References

- [Review checklist](./references/review-checklist.md)
