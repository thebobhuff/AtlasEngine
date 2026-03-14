---
description: Run the post-release operate phase by validating production health, checking post-release signals, and closing the loop into future planning.
name: Operate
argument-hint: initiative slug, folder, or shipped release target
agent: operations-analyst
tools:
  - search
  - edit
---

# Operate Workflow

The initiative to operate is: ${input:initiative:initiative slug, folder name, or shipped release target}

Use the initiative folder under `ai_docs/ideas/<initiative-slug>/` as the working directory for the operate workflow.

Relevant reusable skills for this workflow:

- [initiative-state-manager](../skills/initiative-state-manager/SKILL.md)
- [handoff-composer](../skills/handoff-composer/SKILL.md)
- [release-safety-pack](../skills/release-safety-pack/SKILL.md)

## Preconditions

- Resolve the initiative folder from the provided input.
- Require completed shipping artifacts before operating proceeds:
  - `SHIP_REPORT.md`
  - `RELEASE_NOTES.md`
  - `RELEASE_VERIFICATION.md`
- Read implementation and planning context that still matters to release health:
  - `PRD.md`
  - `TEST_PLAN.md`
  - `TRACEABILITY.md`
  - `DEPLOYMENT_PLAN.md`
  - `ROLLBACK_PLAN.md`
  - `TEAM_MEMORY.md`
  - `DECISIONS.md`
  - `AGENT_HANDOFFS.md`

## Required Operating Artifacts

Create and maintain:

- `POST_RELEASE_CHECKS.md` using `ai_docs/templates/POST_RELEASE_CHECKS.template.md`
- `OPERATIONS_REPORT.md` using `ai_docs/templates/OPERATIONS_REPORT.template.md`
- `OUTCOME_REVIEW.md` using `ai_docs/templates/OUTCOME_REVIEW.template.md`

## Workflow

1. Confirm release context, target version, and intended outcomes from shipping and planning artifacts.
2. Run or record post-release checks for smoke validation, key user journeys, alerts, metrics, and rollback readiness in `POST_RELEASE_CHECKS.md`.
3. Summarize the observed operational state in `OPERATIONS_REPORT.md`, including health, incidents, actions taken, and the next decision.
4. Compare expected outcomes against observed outcomes in `OUTCOME_REVIEW.md`, including customer feedback, metric movement, and recommended follow-up.
5. Update `TEAM_MEMORY.md`, `DECISIONS.md`, `AGENT_HANDOFFS.md`, `PHASE2_STATE.md`, and `ai_docs/ideas/INDEX.md` with the operate-phase result.

## Operating Rules

- Do not mark the initiative healthy without explicit evidence in the operating artifacts.
- If evidence is incomplete, record the uncertainty and the exact missing checks.
- If the release is degraded or rolled back, leave a precise follow-up recommendation and the most appropriate upstream phase to reopen.
- Treat this phase as the feedback loop that closes the SDLC and informs the next iteration.

## Output Rules

- Produce or revise only operating artifacts and shared memory relevant to the post-release state.
- Reuse strong existing content and update incrementally.
- End by summarizing release health, unresolved issues, and whether the loop is closed or should reopen earlier phases.