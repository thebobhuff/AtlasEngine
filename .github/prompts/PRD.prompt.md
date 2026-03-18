---
description: Run phase 2 of product planning from INTERVIEW.md through research, iterative PRD drafting, PRD review loops, security and release-safety planning, UX, UI, test planning, traceability, and final PLAN.md.
name: PRD
argument-hint: initiative slug, folder, or planning target
agent: product-manager-orchestrator
tools:
  - search
  - edit
  - agent
  - fetch
---

# PRD Phase Workflow

The initiative to plan is: ${input:initiative:initiative slug, folder name, or planning target}

You own phase 2 of the product workflow. Use the initiative folder under `ai_docs/ideas/<initiative-slug>/` as the working directory for all planning artifacts.

Relevant reusable skills for this workflow:

- [initiative-state-manager](../skills/initiative-state-manager/SKILL.md)
- [prd-review-loop-rubric](../skills/prd-review-loop-rubric/SKILL.md)
- [threat-modeling-and-security-triage](../skills/threat-modeling-and-security-triage/SKILL.md)
- [traceability-maintainer](../skills/traceability-maintainer/SKILL.md)
- [handoff-composer](../skills/handoff-composer/SKILL.md)
- [release-safety-pack](../skills/release-safety-pack/SKILL.md)
- [supabase-migration-discipline](../skills/supabase-migration-discipline/SKILL.md)

## Preconditions

- Resolve the initiative folder from the provided input.
- Require `INTERVIEW.md` to exist before starting phase 2.
- If the folder already contains phase-2 artifacts, resume from them instead of restarting.
- Maintain all generated files inside the initiative folder.
- Read `ai_docs/WORKFLOW_PROFILE.md` when it exists and apply its planning mode and force-full triggers.
- Use the markdown templates in `ai_docs/templates/` when creating or normalizing all phase-2 memory files and artifacts.

## Workflow Profile Controls

- If `ai_docs/WORKFLOW_PROFILE.md` does not exist, default to `Full` planning mode.
- Supported planning modes are `Full` and `Fast`.
- `Fast` mode is only valid when the profile allows it and no force-full trigger applies.
- Force `Full` mode when any trigger applies, especially for security, privacy, migration, release-safety, supply-chain, major UX/UI, or unclear repository-risk changes.
- Record the active planning mode and any force-full reason in `PHASE2_STATE.md`.
- Keep AtlasEngine documentation-first: the workflow profile tunes the workflow, but does not replace `ai_docs/WORKFLOW.md`.

## Shared Memory System

Phase 2 uses a shared markdown memory system inside the initiative folder so state is preserved from agent to agent and team to team.

Create and maintain these shared memory files:

- `PHASE2_STATE.md`: overall workflow stage, completion state per artifact, blockers, reopen triggers, and next required actions
- `TEAM_MEMORY.md`: confirmed facts, assumptions, unresolved questions, risks, and cross-agent findings
- `DECISIONS.md`: a running decision log with rationale and impacted artifacts
- `AGENT_HANDOFFS.md`: the current handoff and a running history of handoffs between agents

Memory rules:

- Every agent must inspect the shared memory files before acting.
- Every agent must update the shared memory files after acting.
- If an agent discovers a contradiction, missing dependency, or invalid downstream assumption, it must update `TEAM_MEMORY.md`, `DECISIONS.md`, `AGENT_HANDOFFS.md`, and `PHASE2_STATE.md` as appropriate.
- If a downstream phase reveals a material issue in `PRD.md`, the workflow must reopen the PRD loop instead of continuing with stale artifacts.
- The initiative folder is the durable system of record for the entire phase-2 workflow.
- If `TEAM_MEMORY.md`, `DECISIONS.md`, or `AGENT_HANDOFFS.md` already exist from phase 1, carry them forward and extend them rather than recreating them.
- Keep `ai_docs/ideas/INDEX.md` updated with the initiative's current phase, status, latest artifact, and next action.

Use [initiative-state-manager](../skills/initiative-state-manager/SKILL.md) for resume rules, state normalization, and reopen handling instead of inventing ad hoc phase-state behavior.

## Required Artifacts

Always create and maintain these files during the workflow:

- `PHASE2_STATE.md`
- `TEAM_MEMORY.md`
- `DECISIONS.md`
- `AGENT_HANDOFFS.md`
- `CODEBASE_RESEARCH.md`
- `PRD.md`
- `PRD_NOTES.md`
- `PRD_REVIEW_STATE.md`
- `TEST_PLAN.md`
- `PLAN.md`
- `TRACEABILITY.md`

Create and maintain these files when the active planning mode is `Full` or a force-full trigger applies:

- `RESEARCH.md` when external or market context is needed
- `THREAT_MODEL.md`
- `SECURITY_REVIEW.md`
- `SUPPLY_CHAIN.md`
- `STYLE.md` when user-facing work is affected
- `UX.md` when user flows change materially
- `UI.md` when interface structure changes materially
- `DEPLOYMENT_PLAN.md`
- `ROLLBACK_PLAN.md`
- `MIGRATION_PLAN.md` when schema, data, or compatibility changes are relevant
- `RELEASE_VERIFICATION.md`

## Workflow

### 1. Research Preparation

Owner: `product-manager-orchestrator`

The product manager inspects `INTERVIEW.md`, summarizes the initiative, identifies the main unknowns, decides whether external or market research is needed, and gives focused instructions to the product research agent when that workstream is justified.

Before handing off, initialize or refresh:

- `PHASE2_STATE.md` using `ai_docs/templates/PHASE2_STATE.template.md`
- `TEAM_MEMORY.md` using `ai_docs/templates/TEAM_MEMORY.template.md`
- `DECISIONS.md` using `ai_docs/templates/DECISIONS.template.md`
- `AGENT_HANDOFFS.md` using `ai_docs/templates/AGENT_HANDOFFS.template.md`

Record the current stage as `Research`, the active owner, the main unknowns, and the next required outputs.

### 2. Parallel Research

Owners: `product-researcher` and `senior-engineer`

Run these workstreams in parallel when possible and when they are justified by the active planning mode:

- `product-researcher` researches the web for similar products, summarizes their features, and captures what customers like and dislike about those platforms in `RESEARCH.md` using `ai_docs/templates/RESEARCH.template.md` when external or market context is needed.
- `senior-engineer` researches the codebase, identifies likely implementation locations, relevant files, extension points, constraints, and architectural risks in `CODEBASE_RESEARCH.md` using `ai_docs/templates/CODEBASE_RESEARCH.template.md`.

Both owners must update `TEAM_MEMORY.md` with findings and `AGENT_HANDOFFS.md` with the handoff summary back to the orchestrator.

### 3. Team PRD Drafting

Owner: `prd-writer`

Using `INTERVIEW.md`, `RESEARCH.md`, and `CODEBASE_RESEARCH.md`, coordinate the product team to craft `PRD.md`.

Use `ai_docs/templates/PRD.template.md` as the structure baseline.

The team should use specialist input where relevant, especially:

- `solution-architect`
- `ux-designer`
- `ui-designer`
- `qa-strategist`
- `security-reviewer`
- `platform-release-engineer`
- `data-metrics-analyst`
- `scrum-master`

Update `TEAM_MEMORY.md` with the rationale for major requirement decisions and add any formal tradeoff decisions to `DECISIONS.md`.

### 4. PRD Review Loop

Loop owners:

- Draft and revision owner: `prd-writer`
- Review and scoring owner: `product-manager-orchestrator`
- Specialist revision contributors: `solution-architect`, `senior-engineer`, `ux-designer`, `ui-designer`, `qa-strategist`, `security-reviewer`, `platform-release-engineer`, `data-metrics-analyst`, and `scrum-master` when their input is relevant to the current review notes

The product manager reviews `PRD.md`, scores confidence, and writes revision guidance into `PRD_NOTES.md`.

This is a required loop, not a single review pass.

Rules:

- The PRD must go through at least 2 review cycles.
- It may go through up to 5 review cycles.
- Stop the loop early only if 2 or more cycles are complete and confidence is greater than 90%.
- If confidence is 90% or lower after 5 cycles, stop and clearly state the blocking gaps in `PRD_NOTES.md`.
- Do not proceed to UX, UI, testing, or final planning until this loop has completed.

Use [prd-review-loop-rubric](../skills/prd-review-loop-rubric/SKILL.md) for the scoring rubric, blocker classification, revision-focus format, and pass criteria.

Use this control flow exactly:

1. Draft or revise `PRD.md` using the current inputs.
  Owner: `prd-writer`
  Read `TEAM_MEMORY.md`, `DECISIONS.md`, and `AGENT_HANDOFFS.md` before revising.
2. Review `PRD.md` as the product manager.
  Owner: `product-manager-orchestrator`
3. Write the review outcome to `PRD_NOTES.md`.
  Owner: `product-manager-orchestrator`
  Use `ai_docs/templates/PRD_NOTES.template.md` as the structure baseline.
4. Update `PRD_REVIEW_STATE.md` with:
  Owner: `product-manager-orchestrator`
  Use `ai_docs/templates/PRD_REVIEW_STATE.template.md` as the structure baseline.
  - current cycle number
  - current confidence score
  - pass or fail status
  - blocking gaps
  - required revision focus for the next cycle
    Also update `PHASE2_STATE.md`, `TEAM_MEMORY.md`, and `AGENT_HANDOFFS.md` with the current loop outcome.
5. If fewer than 2 cycles have completed, return to step 1.
  Owner for next action: `prd-writer`
6. If 2 or more cycles have completed and confidence is greater than 90%, exit the loop.
  Owner for exit decision: `product-manager-orchestrator`
7. If confidence is 90% or lower and fewer than 5 cycles have completed, return to step 1.
  Owner for next action: `prd-writer`
8. If 5 cycles have completed and confidence is still 90% or lower, stop the loop, record the unresolved blockers, and do not advance unless the user explicitly accepts the remaining risk.
  Owner for blocked outcome: `product-manager-orchestrator`

Each cycle should build on the prior `PRD.md` and `PRD_NOTES.md` rather than replacing them with a generic rewrite.

If any specialist contributor finds a material contradiction during the loop, update the shared memory files and return to step 1 with explicit revision focus.

### 5. Security And Release-Safety Review

Owners: `security-reviewer`, `platform-release-engineer`, and `github-devops-engineer` when repository automation or PR controls matter

Once `PRD.md` has passed the review loop, and only then, run this review when the active planning mode is `Full` or a force-full trigger applies.

Create:

- `THREAT_MODEL.md` using `ai_docs/templates/THREAT_MODEL.template.md`
- `SECURITY_REVIEW.md` using `ai_docs/templates/SECURITY_REVIEW.template.md`
- `SUPPLY_CHAIN.md` using `ai_docs/templates/SUPPLY_CHAIN.template.md`

Requirements for this step:

- apply [threat-modeling-and-security-triage](../skills/threat-modeling-and-security-triage/SKILL.md) for trust boundaries, abuse cases, mitigations, and residual-risk classification
- use [release-safety-pack](../skills/release-safety-pack/SKILL.md) when deployment, migration, rollback, or release verification concerns appear during planning
- use [supabase-migration-discipline](../skills/supabase-migration-discipline/SKILL.md) when the initiative changes Supabase-managed schema, policies, seed data, or environment promotion assumptions

If this review finds material gaps or unsafe assumptions, update the shared memory files, mark the affected artifact `Needs Revision` in `PHASE2_STATE.md`, and reopen the PRD loop.

### 6. Style Artifact

Owner: `ui-designer`

Once `PRD.md` has passed the review loop, `ui-designer` determines whether an existing visual system is already present.

Skip this step only when the active planning mode is `Fast` and the change does not materially affect a user-facing surface.

Use `INTERVIEW.md`, `CODEBASE_RESEARCH.md`, and the current repo state to inspect likely style sources such as:

- CSS and SCSS files
- Tailwind configuration and theme extensions
- design-token files
- shared UI component styles
- font configuration or theme variables

If a credible style system already exists, derive `STYLE.md` from that evidence and record the source files.

If the repo does not provide enough visual guidance, use the style direction captured during discovery. If that guidance is still insufficient, update the shared memory files, mark discovery or planning as needing clarification, and do not invent a visual language.

Use `ai_docs/templates/STYLE.template.md` as the structure baseline.

Requirements for `STYLE.md`:

- identify whether the style is established, evolving, or net new
- capture fonts, colors, spacing tendencies, surface treatment, and motion guidance when known
- reference the repo files that establish the existing style when applicable
- state explicit open questions instead of guessing when the style direction is underspecified

If `ui-designer` finds a material contradiction between discovered design direction and the existing product style, update the shared memory files and reopen the necessary upstream phase.

### 7. UX Artifact

Owner: `ux-designer`

Once `PRD.md` has passed the review loop, and only then, `ux-designer` creates `UX.md` from `PRD.md`.

Skip this step only when the active planning mode is `Fast` and the change does not materially alter user journeys.

Use `ai_docs/templates/UX.template.md` as the structure baseline.

Requirements for `UX.md`:

- describe the intended user experience clearly
- include Mermaid diagrams for the primary flows, states, and key decision points
- stay consistent with the approved `PRD.md`

If `ux-designer` finds a material gap or contradiction in `PRD.md`, update the shared memory files, mark `PRD.md` as `Needs Revision` in `PHASE2_STATE.md`, and reopen the PRD review loop.

### 8. UI Artifact

Owner: `ui-designer`

After `STYLE.md` and `UX.md` are complete, `ui-designer` creates `UI.md` using `PRD.md`, `STYLE.md`, and `UX.md`.

Skip this step only when the active planning mode is `Fast` and the change does not materially alter interface structure.

Use `ai_docs/templates/UI.template.md` as the structure baseline.

Requirements for `UI.md`:

- use ASCII layouts to describe the interface
- identify screens, components, states, and layout structure
- stay consistent with `PRD.md`, `STYLE.md`, and `UX.md`

If `ui-designer` finds a material contradiction with `PRD.md` or `UX.md`, update the shared memory files and reopen the necessary upstream phase.

### 9. Test Plan

Owner: `test-engineer`

After `PRD.md` and all other required upstream design artifacts for the active planning mode are complete, `test-engineer` creates `TEST_PLAN.md`.

Use `ai_docs/templates/TEST_PLAN.template.md` as the structure baseline.

Use [traceability-maintainer](../skills/traceability-maintainer/SKILL.md) to keep requirement coverage explicit and [quality-gates-executor](../skills/quality-gates-executor/SKILL.md) to keep non-functional validation expectations concrete.

If `test-engineer` finds missing or untestable acceptance criteria, update the shared memory files, record the blocker in `PHASE2_STATE.md`, and reopen the PRD loop.

### 10. Final Delivery Plan And Traceability

Owners: `project-manager` and `product-manager-orchestrator`

After all required artifacts for the active planning mode are complete, `project-manager` and the product manager jointly evaluate the full set of required markdown artifacts and produce `PLAN.md`.

`PLAN.md` should define:

- workstreams
- milestones
- sequencing
- dependencies
- delivery risks
- recommended implementation order

Use `ai_docs/templates/PLAN.template.md` as the structure baseline for `PLAN.md`.

Before creating `PLAN.md`, confirm in `PHASE2_STATE.md` that all required upstream artifacts are complete and not marked `Needs Revision`.

Also create:

- `TRACEABILITY.md` using `ai_docs/templates/TRACEABILITY.template.md`
- `DEPLOYMENT_PLAN.md` using `ai_docs/templates/DEPLOYMENT_PLAN.template.md` when the active planning mode is `Full` or release-safety risk requires it
- `ROLLBACK_PLAN.md` using `ai_docs/templates/ROLLBACK_PLAN.template.md` when the active planning mode is `Full` or release-safety risk requires it
- `MIGRATION_PLAN.md` using `ai_docs/templates/MIGRATION_PLAN.template.md` when schema, data, or compatibility changes are relevant
- `RELEASE_VERIFICATION.md` using `ai_docs/templates/RELEASE_VERIFICATION.template.md` when the active planning mode is `Full` or release-safety risk requires it

Use [traceability-maintainer](../skills/traceability-maintainer/SKILL.md) for requirement mapping and [release-safety-pack](../skills/release-safety-pack/SKILL.md) for deployment, rollback, migration, and release-verification quality.

`TRACEABILITY.md` must include implementation evidence expectations, validation evidence expectations, acceptance owner, and release or migration dependency notes for each requirement.

### 11. Approval Gate

Owner: `product-manager-orchestrator`

After `PLAN.md` is complete, do not immediately continue into implementation planning.

First:

- summarize the completed phase-2 work
- present a markdown table of the final planning artifacts
- ask the user for approval to proceed to implementation planning

Use this structure in the final PRD-phase response:

```md
## Phase 2 Summary
- Initiative: ...
- PRD confidence: ...
- Final status: Ready for implementation planning | Blocked

## Artifact Summary
| Artifact | Status | Purpose |
| --- | --- | --- |
| INTERVIEW.md | Complete | Discovery handoff |
| RESEARCH.md | Complete | Market and product research |
| CODEBASE_RESEARCH.md | Complete | Codebase implementation research |
| PRD.md | Complete | Approved product requirements |
| PRD_NOTES.md | Complete | PRD review notes |
| PRD_REVIEW_STATE.md | Complete | PRD loop state |
| THREAT_MODEL.md | Complete or Not Required | Threat and trust-boundary analysis |
| SECURITY_REVIEW.md | Complete or Not Required | Security findings and decision |
| SUPPLY_CHAIN.md | Complete or Not Required | Dependency and integration risk review |
| STYLE.md | Complete or Not Required | Visual system guidance |
| UX.md | Complete or Not Required | UX specification |
| UI.md | Complete or Not Required | UI specification |
| TEST_PLAN.md | Complete | Validation strategy |
| PLAN.md | Complete | Delivery plan |
| TRACEABILITY.md | Complete | Requirement-to-delivery coverage map |
| DEPLOYMENT_PLAN.md | Complete or Not Required | Release deployment strategy |
| ROLLBACK_PLAN.md | Complete or Not Required | Rollback triggers and steps |
| MIGRATION_PLAN.md | Complete or Not Required | Data and schema change plan |
| RELEASE_VERIFICATION.md | Complete or Not Required | Release readiness checks |

## Approval Request
Approve implementation planning? Reply with approval to continue.
```

Approval rules:

- Do not start implementation planning until the user explicitly approves.
- If the user does not approve, stop and wait for revision instructions.
- If the user approves, immediately transition into implementation planning through the implementation-planning workflow.
- When approval is granted, prefer the implementation handoff if available; otherwise continue with the equivalent implementation-planning instructions used by `/Implementation`.

## Resume Rules

- If any file already exists, inspect it before regenerating it.
- Preserve useful prior work and revise incrementally.
- Do not overwrite a strong artifact with a weaker generic rewrite.
- Keep the initiative folder as the single source of truth for planning memory.
- Resume the PRD loop from `PRD_REVIEW_STATE.md` when it exists.
- If the loop is incomplete, continue from the next required review cycle instead of restarting the PRD from scratch.
- Resume overall workflow state from `PHASE2_STATE.md`, `TEAM_MEMORY.md`, `DECISIONS.md`, and `AGENT_HANDOFFS.md` when they exist.
- If any downstream artifact is marked stale or `Needs Revision`, reopen the required upstream phase instead of continuing linearly.
- Keep the structured summary blocks in `PHASE2_STATE.md` and `AGENT_HANDOFFS.md` current as work progresses.

## Output Rules

- Work artifact by artifact.
- Treat the PRD refinement loop as mandatory workflow state, not advisory guidance.
- Keep each file specific to the current initiative and codebase.
- Do not invent evidence from web research or repository context.
- Explicitly distinguish confirmed facts, assumptions, and unresolved issues.
- Use the current workspace and fetched sources as evidence.
- Treat the shared memory files as required state transfer between agents and teams.
- Treat traceability as a required phase-2 output in every planning mode.
- Treat security review and release-safety planning as required whenever the active mode is `Full` or force-full triggers apply.
- Do not stay in `Fast` mode when new evidence shows the initiative is riskier than first assumed. Promote to `Full`, record the reason, and continue.
- End the PRD phase with the approval gate response instead of assuming automatic continuation.

Begin by locating the initiative folder, reading `INTERVIEW.md`, and launching the research phase.