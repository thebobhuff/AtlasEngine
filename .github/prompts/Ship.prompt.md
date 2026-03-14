---
description: Prepare and run the shipping workflow with semantic versioning, GitHub releases, and PR promotion from dev to staging to main.
name: Ship
argument-hint: initiative slug, folder, or shipping target
agent: github-devops-engineer
tools:
  - search
  - edit
  - agent
---

# Shipping Workflow

The initiative to ship is: ${input:initiative:initiative slug, folder name, or shipping target}

Use the initiative folder under `ai_docs/ideas/<initiative-slug>/` as the working directory for the shipping workflow.

Relevant reusable skills for this workflow:

- [github-hardening-branch-airgap](../skills/github-hardening-branch-airgap/SKILL.md)
- [release-safety-pack](../skills/release-safety-pack/SKILL.md)
- [supabase-migration-discipline](../skills/supabase-migration-discipline/SKILL.md)
- [handoff-composer](../skills/handoff-composer/SKILL.md)

## Branch Flow Assumption

Assume the intended promotion flow is:

- current working branch to `dev`
- `dev` to `staging`
- `staging` to `main`

The final `staging -> main` pull request must be created for human approval and must not be auto-merged.

Use [github-hardening-branch-airgap](../skills/github-hardening-branch-airgap/SKILL.md) for the detailed production-branch protection expectations and [release-safety-pack](../skills/release-safety-pack/SKILL.md) for the release-safety artifact checklist.

Use [supabase-migration-discipline](../skills/supabase-migration-discipline/SKILL.md) when release execution includes Supabase schema promotion, drift handling, staged environment mapping, or controlled `supabase db push` steps.

## Preconditions

- Resolve the initiative folder from the provided input.
- Require completed implementation artifacts before shipping:
  - `TASKS.md`
  - `IMPLEMENTATION_STATE.md`
  - `TEST_REPORT.md`
  - `E2E_REPORT.md`
  - `RETRO.md`
- Require release-safety artifacts before shipping proceeds:
  - `TRACEABILITY.md`
  - `DEPLOYMENT_PLAN.md`
  - `ROLLBACK_PLAN.md`
  - `RELEASE_VERIFICATION.md`
- Require implementation to be in a non-blocked state before shipping proceeds.
- Read `PLAN.md`, `PRD.md`, `TEST_PLAN.md`, `TEAM_MEMORY.md`, `DECISIONS.md`, `AGENT_HANDOFFS.md`, `MIGRATION_PLAN.md`, `SECURITY_REVIEW.md`, and `SUPPLY_CHAIN.md` before making release decisions.

## Required Shipping Artifacts

Create and maintain:

- `SHIP_REPORT.md` using `ai_docs/templates/SHIP_REPORT.template.md`
- `RELEASE_NOTES.md` using `ai_docs/templates/RELEASE_NOTES.template.md`

Keep these artifacts current during shipping:

- `RELEASE_VERIFICATION.md`
- `POST_RELEASE_CHECKS.md` for the operate handoff

## Semantic Versioning

Use the repository `VERSION` file as the semantic version source of truth.

Rules:

- Follow semantic versioning: `MAJOR.MINOR.PATCH`
- Use `MAJOR` for breaking changes
- Use `MINOR` for backward-compatible feature delivery
- Use `PATCH` for backward-compatible fixes only
- Update the version to the next correct value before release preparation completes
- Use the Git tag format `vX.Y.Z`

If the repository has other obvious version files, keep them aligned with `VERSION` when the mapping is clear.

## GitHub Releases

Use GitHub Releases as the release mechanism.

Rules:

- Prepare `RELEASE_NOTES.md` before the final release step.
- Use the semantic version tag as the release tag.
- If the GitHub CLI is available, prepare the release flow with it.
- If release automation cannot be completed in the environment, leave a precise manual next step in `SHIP_REPORT.md`.

## Agent Responsibilities During Shipping

- `github-devops-engineer` owns the GitHub-specific pipeline work: pull request preparation, branch promotion, required checks, GitHub CLI usage, branch protection constraints, and GitHub Release execution.
- `release-manager` remains available as a specialist for release-readiness review, semantic versioning judgment, and final release-status framing when extra coordination is needed.
- `platform-release-engineer` supports operational readiness, deployment safety, observability, and rollback concerns.
- `operations-analyst` owns the post-release operate phase after shipping is complete.

## Pull Request Workflow

Use the repository pull request template in `.github/pull_request_template.md`.

Shipping steps:

1. Verify shipping readiness from the implementation and test artifacts.
2. Determine and record the semantic version bump.
3. Confirm `DEPLOYMENT_PLAN.md`, `ROLLBACK_PLAN.md`, `MIGRATION_PLAN.md`, `TRACEABILITY.md`, and `RELEASE_VERIFICATION.md` are current.
4. Update `VERSION`, `RELEASE_NOTES.md`, and `SHIP_REPORT.md`.
5. Prepare `POST_RELEASE_CHECKS.md` for the operate handoff.
6. Create or prepare the PR flow from the current work into `dev` and merge it when ready.
7. Create or prepare the PR flow from `dev` to `staging` and merge it when ready.
8. Create the final PR from `staging` to `main` for human approval.
9. Do not auto-merge the final PR.
10. After `staging -> main` is approved and merged, finalize the GitHub Release using the new semantic version.
11. Hand off to `/Operate` with the shipped version, release state, and prepared post-release checks.

The GitHub-facing parts of steps 6 through 10 should be handled through the `github-devops-engineer` role.

## Shared Memory Updates

During shipping:

- update `TEAM_MEMORY.md` with release risks and shipping notes
- add versioning and release decisions to `DECISIONS.md`
- update `AGENT_HANDOFFS.md` with shipping handoff state
- update `PHASE2_STATE.md` to reflect shipping readiness or blocked release state
- update `ai_docs/ideas/INDEX.md` with the initiative's current phase, status, latest artifact, and next action

## Output Rules

- Produce or revise only shipping artifacts and versioning artifacts relevant to release preparation.
- Treat release-safety verification and operate handoff preparation as required shipping outputs.
- Reuse strong prior content instead of replacing it with a generic rewrite.
- Keep the shipping record specific to the current initiative and repo state.
- End by summarizing the target version, PR progression status, release status, whether human approval is pending, and whether the initiative is ready for `/Operate`.