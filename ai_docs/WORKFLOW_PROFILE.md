# Workflow Profile

## Purpose

This profile tunes AtlasEngine for the current repository without replacing the workflow guide or repo-native operational truth.

## Planning Mode
- Default planning mode: Full
- Fast path allowed: Yes
- Fast path intent: Use for bounded, low-risk changes that do not need the full phase-2 artifact set.

## Fast Path Minimum Artifacts
- PHASE2_STATE.md
- TEAM_MEMORY.md
- DECISIONS.md
- AGENT_HANDOFFS.md
- RESEARCH.md when external or market context is needed
- CODEBASE_RESEARCH.md
- PRD.md
- PRD_NOTES.md
- PRD_REVIEW_STATE.md
- TEST_PLAN.md
- PLAN.md
- TRACEABILITY.md

## Force Full Planning When Any Apply
- New product surface or major workflow introduction
- Authentication, authorization, privacy, or compliance-sensitive change
- Schema, migration, or compatibility-risk change
- External integration or supply-chain risk that needs explicit review
- Material UX or UI redesign
- Release, rollback, or staged rollout risk
- Repository constraints are still unclear after codebase research

## Full Path Required Artifacts
- THREAT_MODEL.md
- SECURITY_REVIEW.md
- SUPPLY_CHAIN.md
- STYLE.md when user-facing work is affected
- UX.md when user flows change materially
- UI.md when interface structure changes materially
- DEPLOYMENT_PLAN.md
- ROLLBACK_PLAN.md
- MIGRATION_PLAN.md when schema, data, or compatibility changes are relevant
- RELEASE_VERIFICATION.md

## Implementation Expectations
- Keep TRACEABILITY.md current with implementation evidence, validation evidence, acceptance owner, and release or migration dependencies.
- Keep structured summary blocks current in phase state and handoff artifacts.
- Run /WorkflowHealth after meaningful workflow-customization changes and before declaring a transplanted repo workflow ready.

## Notes
- This file is intentionally lightweight. It tunes AtlasEngine behavior; it does not replace ai_docs/WORKFLOW.md.