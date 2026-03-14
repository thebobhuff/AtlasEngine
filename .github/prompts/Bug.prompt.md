---
description: Pull open bug issues from the repository's GitHub tracker, triage them into executable waves, fix them through the engineering loop, and produce a closure-ready bug report.
name: Bug
argument-hint: bug run name, repo, issue filter, or remediation scope
agent: project-manager
tools:
  - search
  - edit
  - agent
  - browser
  - fetch
---

# Bug Remediation Workflow

The bug remediation target is: ${input:scope:bug run name, repo, issue filter, or remediation scope}

Use a bug-run folder under `ai_docs/bugs/<bug-run-slug>/` as the working directory for all bug intake, execution, and validation artifacts.

Relevant reusable skills for this workflow:

- [quality-gates-executor](../skills/quality-gates-executor/SKILL.md)
- [browser-validation-strategy](../skills/browser-validation-strategy/SKILL.md)
- [traceability-maintainer](../skills/traceability-maintainer/SKILL.md)
- [handoff-composer](../skills/handoff-composer/SKILL.md)
- [release-safety-pack](../skills/release-safety-pack/SKILL.md)

## Preconditions

- Resolve the repository from local git metadata before attempting GitHub issue intake.
- Default issue scope to open GitHub issues labeled `bug` unless the user input explicitly narrows or broadens the filter.
- If the bug-run folder already exists, resume from it instead of restarting.
- Maintain all generated files inside the bug-run folder.
- Use the markdown templates in `ai_docs/templates/` when creating or normalizing bug workflow artifacts.
- If GitHub issues cannot be retrieved because the repository cannot be resolved, authentication is missing, or issue access is unavailable, mark the workflow blocked in `BUG_STATE.md`, record the exact blocker in `BUG_INTAKE.md`, and stop before implementation planning.

## Required Artifacts

Create and maintain these files during the workflow:

- `BUG_STATE.md` using `ai_docs/templates/BUG_STATE.template.md`
- `BUG_INTAKE.md` using `ai_docs/templates/BUG_INTAKE.template.md`
- `TEAM_MEMORY.md` using `ai_docs/templates/TEAM_MEMORY.template.md`
- `DECISIONS.md` using `ai_docs/templates/DECISIONS.template.md`
- `AGENT_HANDOFFS.md` using `ai_docs/templates/AGENT_HANDOFFS.template.md`
- `TASKS.md` using `ai_docs/templates/TASKS.template.md`
- `IMPLEMENTATION_STATE.md` using `ai_docs/templates/IMPLEMENTATION_STATE.template.md`
- `TEST_REPORT.md` using `ai_docs/templates/TEST_REPORT.template.md`
- `E2E_REPORT.md` using `ai_docs/templates/E2E_REPORT.template.md` when browser validation is relevant
- `BUG_FIX_REPORT.md` using `ai_docs/templates/BUG_FIX_REPORT.template.md`
- `RETRO.md` using `ai_docs/templates/RETRO.template.md` after remediation completes

Keep these artifacts current when bug fixes materially change rollout or validation expectations:

- `TRACEABILITY.md` when requirement-level traceability exists for the impacted behavior
- `DEPLOYMENT_PLAN.md`
- `ROLLBACK_PLAN.md`
- `MIGRATION_PLAN.md`
- `RELEASE_VERIFICATION.md`

## Workflow

### 1. Bug-Run Initialization

Owner: `project-manager`

Resolve or create the bug-run folder, initialize the shared memory files, and record the intended issue filter, repository, and current owner.

Update these files immediately:

- `BUG_STATE.md`
- `TEAM_MEMORY.md`
- `DECISIONS.md`
- `AGENT_HANDOFFS.md`

Record the current phase as `Issue Intake`, the active owner, and the exact issue query to use.

### 2. GitHub Issue Intake

Owners: `github-devops-engineer` and `project-manager`

Pull the repository's GitHub issues using repository evidence first and GitHub issue tooling second.

Issue intake rules:

- Resolve the GitHub owner and repository name from local git metadata when possible.
- Prefer open issues labeled `bug`.
- Capture issue number, title, URL, labels, status, reproduction details, expected behavior, actual behavior, severity, affected area, and any existing workaround.
- Mark each issue as one of `Actionable`, `Needs Clarification`, `Duplicate`, `Deferred`, or `Blocked Externally`.
- Do not silently drop issues that are unclear or duplicated; record the disposition in `BUG_INTAKE.md`.
- If the environment can read GitHub issues but not modify them, continue with remediation planning and record that closure steps are manual.

Use `BUG_INTAKE.md` as the durable intake ledger for the run.

### 3. Triage And Wave Planning

Owner: `project-manager`

Turn actionable issues into executable engineering work.

Triage rules:

- Group related bugs when they clearly share the same root cause or subsystem.
- Separate unrelated bugs into different waves when fixing them together would obscure causality or over-expand the blast radius.
- Prioritize by severity, customer impact, reproducibility, and dependency ordering.
- Use `[S]` for tasks that must be done in order and `[P]` for independent fixes that can be worked concurrently.
- Keep issue-to-task mapping explicit in both `BUG_INTAKE.md` and `TASKS.md`.

Also build a dependency graph in `TASKS.md` using Mermaid.

Use [traceability-maintainer](../skills/traceability-maintainer/SKILL.md) when the affected behavior maps back to existing requirements, tests, or release notes.

### 4. Engineering Team

Core remediation team:

- `project-manager` as remediation orchestrator
- `github-devops-engineer` for GitHub issue intake and closure context
- `developer` for scoped bug fixes
- `senior-engineer` for root-cause analysis and technical tradeoffs
- `solution-architect` when integration boundaries or architectural faults are involved
- `test-engineer` for test-loop enforcement
- `e2e-tester` for browser-based validation when the bug affects user-facing flows

Supporting specialists when needed:

- `qa-strategist`
- `product-manager-orchestrator`
- `platform-release-engineer`

### 5. Build Execution Loop

Owner: `project-manager`

Use the same engineering loop shape as `/Implementation`, but scoped to the actionable bugs in this run.

Execution control flow:

1. Start the next eligible wave.
2. Dispatch `[P]` tasks in that wave to subagents in parallel where appropriate.
3. Dispatch `[S]` tasks in dependency order.
4. After each task completion, update `TASKS.md`, `IMPLEMENTATION_STATE.md`, and `BUG_STATE.md`.
5. If a task is blocked, record the blocker and decide whether other parallel work can continue.
6. Do not mark a wave complete until its issue coverage, testing loop, and required quality gates have passed.

### 6. Testing Loop

Owner: `test-engineer`

Testing is mandatory during bug remediation.

Testing rules:

- Run relevant tests after meaningful code changes and at minimum at the end of each wave.
- Prefer the narrowest test scope that proves the bug is fixed, then run broader regression coverage appropriate to the risk.
- If tests fail, the remediation loop is not complete.
- Failed tests must return work to the appropriate implementation task owners.
- Continue the cycle of implement -> test -> fix -> retest until the wave passes or 3 attempts have been used.
- Maintain `TEST_REPORT.md` as the durable test artifact for the current wave.
- After each attempt, record test outcomes, failures, and required fixes in `TEST_REPORT.md`, `IMPLEMENTATION_STATE.md`, `BUG_STATE.md`, `TASKS.md`, and `TEAM_MEMORY.md`.
- If the wave passes within 3 attempts, mark the test loop passed and continue.
- If the wave still fails after 3 attempts, mark the wave blocked, stop automatic retries, and use `TEST_REPORT.md` as the repair guide for the failing items.

Use [quality-gates-executor](../skills/quality-gates-executor/SKILL.md) for the applicable-gate checklist, `Not Applicable` rules, and failure handoff format.

Use this control flow exactly:

1. Run the relevant tests for the completed tasks or current wave.
2. Write or update `TEST_REPORT.md` with the result of the attempt.
3. Run the required quality gates and record their status in `IMPLEMENTATION_STATE.md` and `BUG_STATE.md`.
4. If the tests and quality gates pass, mark the wave test status as passed.
5. If the tests or quality gates fail and fewer than 3 attempts have been used, send the failing items back for fixes and retest.
6. If the tests or quality gates fail and 3 attempts have been used, mark the wave blocked and stop the test loop.

### 7. End-to-End Validation Loop

Owner: `e2e-tester`

Run browser-based validation when the fixed bugs affect user-facing or browser-accessible flows.

Use [browser-validation-strategy](../skills/browser-validation-strategy/SKILL.md) for scenario selection, key-path prioritization, and failure evidence.

Use this control flow exactly:

1. Run the required browser scenarios derived from the bug reproductions, expected behavior, and relevant regression paths.
2. Write or update `E2E_REPORT.md` with scenario coverage and results.
3. If E2E passes, mark the covered issues validated.
4. If E2E fails and fewer than 3 attempts have been used, send the failing scenarios back for fixes and rerun E2E.
5. If E2E fails and 3 attempts have been used, mark the affected issues blocked and stop the E2E loop.

If browser validation is not relevant for a bug wave, record `Not Applicable` with rationale in `BUG_STATE.md` and `BUG_FIX_REPORT.md`.

### 8. Release-Safety Check

Owners: `project-manager` and `platform-release-engineer`

If any bug fix changes rollout behavior, data handling, migrations, or operational risk, update the applicable release-safety artifacts before closeout.

Use [release-safety-pack](../skills/release-safety-pack/SKILL.md) to keep deployment, rollback, migration, and verification expectations current.

### 9. Bug Closure Report

Owners: `project-manager` and `github-devops-engineer`

Produce `BUG_FIX_REPORT.md` as the closure-ready summary for the run.

For each issue, record:

- issue number and title
- disposition
- implemented fix summary
- files or subsystems changed when known
- tests run
- E2E status when applicable
- release-safety impact
- closure recommendation: `Ready To Close`, `Needs Follow-Up`, `Deferred`, `Blocked`, or `Needs Clarification`

If the environment cannot write back to GitHub, include the exact manual closure or follow-up action needed per issue.

### 10. Completion Gate

The workflow is complete only when:

- every actionable issue is fixed, deferred with rationale, blocked with rationale, or explicitly marked as needing clarification
- `BUG_INTAKE.md` reflects the latest triage disposition for all retrieved issues
- `TASKS.md` and `IMPLEMENTATION_STATE.md` reflect the final execution state
- `TEST_REPORT.md` reflects the final passing state or the blocked failures for the most recent wave
- `E2E_REPORT.md` reflects the final passing state or explicitly states why browser validation was not applicable
- `BUG_FIX_REPORT.md` is complete enough to drive issue closure or follow-up
- `RETRO.md` captures bug remediation lessons and workflow improvements

### 11. Shared Memory Updates

Update the shared bug-run memory files throughout remediation:

- add reproduction findings, root-cause hypotheses, and blockers to `TEAM_MEMORY.md`
- add important remediation decisions to `DECISIONS.md`
- update `AGENT_HANDOFFS.md` with issue intake, repair, test, and closure handoff state
- update `BUG_STATE.md` to record the current phase, wave status, blockers, and next required actions

## Output Rules

- Keep the workflow grounded in actual GitHub issue evidence and current repository context.
- Reuse strong prior content instead of replacing it with a generic rewrite.
- Keep issue dispositions explicit even when work is deferred or blocked.
- Organize remediation around waves, `[S]` and `[P]` task modes, and explicit validation loops.
- Parallel bug fixes should be delegated to subagents when appropriate.
- Serial fixes should be worked in order.
- Do not close or recommend closure for an issue unless the fix and required validation evidence are recorded.

Finish by summarizing the issue intake status, current wave state, remaining blockers, which bugs are ready to close, and which bugs still need clarification or follow-up.