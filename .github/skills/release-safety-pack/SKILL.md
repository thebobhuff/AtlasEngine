---
name: release-safety-pack
description: 'Prepare and review deployment safety, rollback readiness, migration risk, release verification, and operate handoff artifacts. Use when updating DEPLOYMENT_PLAN.md, ROLLBACK_PLAN.md, MIGRATION_PLAN.md, RELEASE_VERIFICATION.md, SHIP_REPORT.md, or POST_RELEASE_CHECKS.md.'
argument-hint: 'initiative folder, shipping scope, or release safety review'
user-invocable: true
---

# Release Safety Pack

Use this skill when release readiness depends on deployment, rollback, migration, verification, or post-release checks.

## When To Use

- During phase-2 release-safety planning
- Before `/Ship` proceeds
- When implementation changes rollout, migration, or rollback assumptions
- When preparing the operate handoff

## Artifacts Covered

- `DEPLOYMENT_PLAN.md`
- `ROLLBACK_PLAN.md`
- `MIGRATION_PLAN.md`
- `RELEASE_VERIFICATION.md`
- `SHIP_REPORT.md`
- `POST_RELEASE_CHECKS.md`

## Procedure

1. Confirm the current implementation and traceability state before reviewing release safety.
2. Define the rollout strategy: standard, flagged, phased, canary, or other.
3. Ensure `DEPLOYMENT_PLAN.md` contains concrete sequencing, owners, and verification steps.
4. Ensure `ROLLBACK_PLAN.md` contains explicit triggers, steps, and post-rollback verification.
5. Ensure `MIGRATION_PLAN.md` exists when schema, data, or compatibility changes matter.
6. Update `RELEASE_VERIFICATION.md` with pre-release, release-time, and post-release checks.
7. Ensure `SHIP_REPORT.md` reflects whether release-safety artifacts are current and whether blockers remain.
8. Prepare `POST_RELEASE_CHECKS.md` so operate work starts with explicit production checks instead of generic follow-up.

## Blocker Rules

- Block release if rollback cannot be explained concretely.
- Block release if migration risk exists but no migration plan or compatibility strategy exists.
- Block release if release verification is missing critical checks for the rollout strategy.
- Treat manual-only release steps as acceptable only when documented precisely and intentionally.

## References

- [Release safety checklist](./references/release-safety-checklist.md)
